# CnG Architecture với Minimal LLM Agent

## 1. Kiến trúc tổng quan

```
CnG Agent (LLM Agent) - Brain/Router
├── Product Workflow (SequentialAgent) - Deterministic
├── Cart Workflow (SequentialAgent) - Deterministic  
└── Order Workflow (SequentialAgent) - Deterministic
```

## 2. CnG Agent - LLM Router duy nhất

```python
from google.adk.agents import LlmAgent
from google.adk.agents import SequentialAgent, ParallelAgent

class CnGAgent(LlmAgent):
    """CnG Agent - LLM duy nhất để phân tích intent và điều phối"""
    
    def __init__(self):
        super().__init__(
            model="gemini-2.0-flash-001",
            name="cng_agent",
            description="E-commerce assistant coordinator",
            instruction="""
            Bạn là CnG Agent - điều phối viên chính của hệ thống mua sắm.
            
            NHIỆM VỤ CỦA BẠN:
            1. Phân tích ý định người dùng
            2. Xác định workflow phù hợp
            3. Chuyển control đến workflow tương ứng
            
            KHÔNG tự xử lý chi tiết - chỉ phân tích và điều phối.
            
            WORKFLOWS AVAILABLE:
            - product_workflow: Tìm kiếm và xem chi tiết sản phẩm
            - cart_workflow: Quản lý giỏ hàng (thêm, xóa, cập nhật)  
            - order_workflow: Xử lý đặt hàng và thanh toán
            
            DECISION LOGIC:
            - Nếu user hỏi về sản phẩm/tìm kiếm -> product_workflow
            - Nếu user muốn thêm vào giỏ/xem giỏ -> cart_workflow
            - Nếu user muốn thanh toán/đặt hàng -> order_workflow
            
            Sử dụng transfer để chuyển control:
            <anythingllm:transfer>workflow_name</anythingllm:transfer>
            """,
            sub_agents=[
                create_product_workflow(),
                create_cart_workflow(),
                create_order_workflow()
            ]
        )
```

## 3. Product Workflow - Không dùng LLM

```python
def create_product_workflow():
    """Product workflow - deterministic steps"""
    
    # Step tools - pure functions
    async def extract_search_params(context):
        """Extract search parameters from user message"""
        user_message = context.new_message.content
        # Simple pattern matching hoặc NLP nhẹ
        return {
            "query": extract_query(user_message),
            "filters": extract_filters(user_message)
        }
    
    async def search_products(context):
        """Execute product search via API"""
        params = context.state.get("search_params", {})
        result = await api_client.search_products(**params)
        context.state["search_results"] = result
        return result
    
    async def format_search_results(context):
        """Format results for display"""
        results = context.state.get("search_results", {})
        formatted = format_products_for_display(results)
        return Event(
            author="product_workflow",
            content=types.Content(
                role="assistant",
                parts=[types.Part(text=formatted)]
            )
        )
    
    # Sequential workflow - no LLM
    product_workflow = SequentialAgent(
        name="product_workflow",
        description="Handle product search and details",
        sub_agents=[
            FunctionAgent(extract_search_params, name="extract_params"),
            FunctionAgent(search_products, name="search_api"),
            FunctionAgent(format_search_results, name="format_results")
        ]
    )
    
    return product_workflow
```

## 4. Cart Workflow - Không dùng LLM

```python
def create_cart_workflow():
    """Cart management workflow - deterministic"""
    
    async def validate_cart_action(context):
        """Validate cart operation"""
        user_message = context.new_message.content
        action = extract_cart_action(user_message)  # ADD, REMOVE, UPDATE, VIEW
        
        if action == "ADD":
            product_id = extract_product_id(user_message)
            quantity = extract_quantity(user_message)
            
            if not product_id:
                return Event(
                    author="cart_workflow",
                    content=types.Content(
                        role="assistant",
                        parts=[types.Part(text="Vui lòng chỉ định sản phẩm cần thêm vào giỏ.")]
                    )
                )
            
            context.state["cart_action"] = {
                "type": "ADD",
                "product_id": product_id,
                "quantity": quantity
            }
        
        return None
    
    async def ensure_cart_exists(context):
        """Ensure cart exists or create new one"""
        cart_id = context.state.get("cart_id")
        
        if not cart_id:
            result = await api_client.create_cart(is_guest=True)
            cart_id = result.get("cart_id")
            context.state["cart_id"] = cart_id
            
        return cart_id
    
    async def execute_cart_action(context):
        """Execute the cart operation"""
        action = context.state.get("cart_action", {})
        cart_id = context.state.get("cart_id")
        
        if action["type"] == "ADD":
            result = await api_client.add_to_cart(
                cart_id=cart_id,
                product_id=action["product_id"],
                quantity=action["quantity"]
            )
        elif action["type"] == "REMOVE":
            result = await api_client.remove_from_cart(
                cart_id=cart_id,
                item_id=action["item_id"]
            )
        # ... other actions
        
        context.state["cart_result"] = result
        return result
    
    async def format_cart_response(context):
        """Format cart operation result"""
        result = context.state.get("cart_result", {})
        action = context.state.get("cart_action", {})
        
        message = format_cart_message(action["type"], result)
        
        return Event(
            author="cart_workflow",
            content=types.Content(
                role="assistant",
                parts=[types.Part(text=message)]
            )
        )
    
    # Sequential workflow
    cart_workflow = SequentialAgent(
        name="cart_workflow",
        description="Manage shopping cart operations",
        sub_agents=[
            FunctionAgent(validate_cart_action, name="validate"),
            FunctionAgent(ensure_cart_exists, name="ensure_cart"),
            FunctionAgent(execute_cart_action, name="execute"),
            FunctionAgent(format_cart_response, name="format")
        ]
    )
    
    return cart_workflow
```

## 5. Order Workflow - Không dùng LLM

```python
def create_order_workflow():
    """Order processing workflow - deterministic"""
    
    # Parallel validation steps
    async def validate_cart_contents(context):
        """Validate cart has items"""
        cart_id = context.state.get("cart_id")
        cart_info = await api_client.get_cart_info(cart_id)
        
        if not cart_info["items"]:
            raise ValueError("Cart is empty")
            
        context.state["cart_validated"] = True
        return cart_info
    
    async def validate_shipping_info(context):
        """Validate shipping information"""
        user_profile = context.state.get("user_profile", {})
        
        if not user_profile.get("shipping_address"):
            return Event(
                author="order_workflow",
                content=types.Content(
                    role="assistant",
                    parts=[types.Part(text="Vui lòng cung cấp địa chỉ giao hàng.")]
                )
            )
        
        context.state["shipping_validated"] = True
        return user_profile["shipping_address"]
    
    async def validate_payment_method(context):
        """Validate payment method"""
        payment_method = context.state.get("payment_method")
        
        if not payment_method:
            return Event(
                author="order_workflow",
                content=types.Content(
                    role="assistant",
                    parts=[types.Part(text="Vui lòng chọn phương thức thanh toán.")]
                )
            )
        
        context.state["payment_validated"] = True
        return payment_method
    
    # Parallel validation
    validation_parallel = ParallelAgent(
        name="order_validation",
        sub_agents=[
            FunctionAgent(validate_cart_contents, name="validate_cart"),
            FunctionAgent(validate_shipping_info, name="validate_shipping"),
            FunctionAgent(validate_payment_method, name="validate_payment")
        ]
    )
    
    # Sequential order processing
    async def create_order(context):
        """Create order if all validations pass"""
        if all([
            context.state.get("cart_validated"),
            context.state.get("shipping_validated"),
            context.state.get("payment_validated")
        ]):
            order_data = prepare_order_data(context)
            result = await api_client.create_order(order_data)
            context.state["order_result"] = result
            return result
        else:
            raise ValueError("Validation failed")
    
    async def process_payment(context):
        """Process payment for order"""
        order_id = context.state["order_result"]["order_id"]
        payment_result = await api_client.process_payment(order_id)
        context.state["payment_result"] = payment_result
        return payment_result
    
    async def send_confirmation(context):
        """Send order confirmation"""
        order_id = context.state["order_result"]["order_id"]
        confirmation = format_order_confirmation(context)
        
        return Event(
            author="order_workflow",
            content=types.Content(
                role="assistant",
                parts=[types.Part(text=confirmation)]
            )
        )
    
    # Complete order workflow
    order_workflow = SequentialAgent(
        name="order_workflow",
        description="Process order and payment",
        sub_agents=[
            validation_parallel,  # Parallel validation
            FunctionAgent(create_order, name="create_order"),
            FunctionAgent(process_payment, name="process_payment"),
            FunctionAgent(send_confirmation, name="send_confirmation")
        ]
    )
    
    return order_workflow
```

## 6. Helper Functions - No LLM

```python
# Pure functions for data extraction and formatting
def extract_query(message: str) -> str:
    """Extract search query from message"""
    # Simple pattern matching
    patterns = [
        r"tìm kiếm (.+)",
        r"tìm (.+)",
        r"search for (.+)",
        r"muốn mua (.+)"
    ]
    
    for pattern in patterns:
        match = re.search(pattern, message.lower())
        if match:
            return match.group(1)
    
    # Default to full message
    return message

def extract_cart_action(message: str) -> str:
    """Determine cart action from message"""
    message_lower = message.lower()
    
    if any(word in message_lower for word in ["thêm", "add", "cho vào"]):
        return "ADD"
    elif any(word in message_lower for word in ["xóa", "remove", "bỏ"]):
        return "REMOVE"
    elif any(word in message_lower for word in ["xem giỏ", "view cart", "giỏ hàng"]):
        return "VIEW"
    elif any(word in message_lower for word in ["cập nhật", "update", "thay đổi"]):
        return "UPDATE"
    
    return "UNKNOWN"

def format_products_for_display(results: dict) -> str:
    """Format product search results"""
    if not results.get("products"):
        return "Không tìm thấy sản phẩm phù hợp."
    
    formatted = f"Tìm thấy {results['total_results']} sản phẩm:\n\n"
    
    for product in results["products"]:
        formatted += f"• {product['name']}\n"
        formatted += f"  Giá: {product['price']:,}đ"
        if product.get('discount_percentage'):
            formatted += f" (giảm {product['discount_percentage']}%)"
        formatted += f"\n  SKU: {product['sku']}\n\n"
    
    return formatted
```

## 7. Function Agent Wrapper

```python
from google.adk.agents import BaseAgent
from google.adk.events import Event
from google.genai import types

class FunctionAgent(BaseAgent):
    """Wrapper để biến function thành agent"""
    
    def __init__(self, func, name=None, description=None):
        self.func = func
        super().__init__(
            name=name or func.__name__,
            description=description or func.__doc__
        )
    
    async def _run_async_impl(self, context):
        try:
            result = await self.func(context)
            
            # Nếu function trả về Event, yield trực tiếp
            if isinstance(result, Event):
                yield result
            else:
                # Nếu không, store vào state
                context.state[f"{self.name}_result"] = result
                
        except Exception as e:
            yield Event(
                author=self.name,
                content=types.Content(
                    role="system",
                    parts=[types.Part(text=f"Error in {self.name}: {str(e)}")]
                )
            )
```

## 8. Ưu điểm của kiến trúc này

1. **Minimal LLM Usage**: Chỉ CnG dùng LLM để routing
2. **Deterministic Workflows**: Các workflow không random, dễ kiểm soát
3. **Faster Execution**: Không cần chờ LLM inference cho mỗi step
4. **Better Error Handling**: Lỗi dễ debug và xử lý
5. **Cost Effective**: Giảm chi phí API calls đến LLM
6. **Maintainable**: Logic rõ ràng, dễ update

## 9. Usage Example

```python
# Initialize system
cng_agent = CnGAgent()

# Create runner
runner = Runner(
    agent=cng_agent,
    app_name="ecommerce_assistant",
    session_service=InMemorySessionService()
)

# Process user request
async for event in runner.run_async(
    user_id="user_123",
    session_id="session_456",
    new_message="Tôi muốn tìm điện thoại Samsung"
):
    if event.is_final_response():
        print(event.content.parts[0].text)

# CnG sẽ:
# 1. Phân tích intent -> "search products"
# 2. Transfer control -> product_workflow
# 3. product_workflow thực hiện deterministic steps
# 4. Trả về kết quả formatted
```

## 10. State Management

```python
# Shared state giữa workflows
class WorkflowContext:
    def __init__(self):
        self.state = {
            'cart_id': None,
            'user_profile': {},
            'current_workflow': None,
            'last_search': None,
            'order_history': []
        }
    
    def update(self, key: str, value: Any):
        self.state[key] = value
    
    def get(self, key: str, default=None):
        return self.state.get(key, default)
```

Kiến trúc này tối ưu hóa performance và accuracy bằng cách:
- Chỉ dùng LLM cho việc routing (CnG agent)
- Workflows deterministic cho các tác vụ cụ thể
- Giảm latency và cost
- Tăng reliability và maintainability
