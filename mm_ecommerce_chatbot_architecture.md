# MM E-commerce Chatbot Architecture - Simplified ADK Implementation

## 1. Tổng quan kiến trúc với Google ADK

### 1.1 Simplified Hierarchy Design
```
Root Agent (LLM - Primary Intent Router)
└── CnG Agent (LLM - E-commerce Coordinator)
    ├── Product Workflow (SequentialAgent - Deterministic)
    ├── Cart Workflow (SequentialAgent - Deterministic)
    ├── Order Workflow (SequentialAgent - Deterministic)
    ├── User Account Workflow (SequentialAgent - Deterministic)
    ├── Promotion Workflow (SequentialAgent - Deterministic)
    └── Support Workflow (SequentialAgent - Deterministic)
```

### 1.2 Google ADK Components Used
- **LlmAgent**: Cho Root và CnG agents
- **SequentialAgent**: Cho workflow steps tuần tự
- **ParallelAgent**: Cho concurrent operations
- **FunctionWrapper**: Convert functions thành agents
- **SessionService**: Custom implementation với storage tiers
- **Runner**: Main execution engine với streaming
- **EventSystem**: Async communication

## 2. Root Agent Implementation

### 2.1 Root Agent Structure
```python
from google.adk.agents import LlmAgent
from google.adk.agents.patterns import transfer_to_agent

class RootAgent(LlmAgent):
    """Root Agent - Primary coordinator"""
    
    def __init__(self):
        super().__init__(
            model="gemini-2.0-flash-001",
            name="root_agent",
            description="Primary router for MM E-commerce chatbot",
            instruction=ROOT_AGENT_INSTRUCTION,
            sub_agents=[cng_agent],
            tools=[
                memorize,
                get_memory,
                analyze_context
            ]
        )
```

### 2.2 Root Agent Instruction
```
ROOT_AGENT_INSTRUCTION = """
Bạn là Root Agent - điều phối viên chính của hệ thống MM E-commerce.

NHIỆM VỤ:
1. Phân tích và hiểu ý định người dùng
2. Xử lý các câu hỏi chung về công ty, chính sách
3. Chuyển các yêu cầu mua sắm đến CnG Agent

ROUTING LOGIC:
- Nếu là câu hỏi về mua sắm, sản phẩm, đơn hàng → Transfer to cng_agent
- Nếu là câu hỏi chung → Trả lời trực tiếp từ knowledge base

Sử dụng <anythingllm:transfer>cng_agent</anythingllm:transfer> khi cần chuyển sang e-commerce flow.
"""
```

## 3. CnG Agent với ADK Pattern

### 3.1 CnG Agent Implementation
```python
class CnGAgent(LlmAgent):
    """E-commerce coordinator agent"""
    
    def __init__(self):
        super().__init__(
            model="gemini-2.0-flash-001",
            name="cng_agent",
            description="E-commerce assistant coordinator",
            instruction=CNG_INSTRUCTION,
            sub_agents=[
                create_product_workflow(),
                create_cart_workflow(),
                create_order_workflow(),
                create_account_workflow(),
                create_promotion_workflow(),
                create_support_workflow()
            ]
        )
```

### 3.2 CnG Agent Instruction
```
CNG_INSTRUCTION = """
Bạn là CnG Agent - điều phối viên e-commerce.

NHIỆM VỤ:
1. Phân tích ý định mua sắm cụ thể
2. Chọn workflow phù hợp
3. Chuyển control đến workflow tương ứng

WORKFLOWS:
- product_workflow: Tìm kiếm và xem sản phẩm
- cart_workflow: Quản lý giỏ hàng
- order_workflow: Đặt hàng và thanh toán
- account_workflow: Quản lý tài khoản
- promotion_workflow: Khuyến mãi và mã giảm giá
- support_workflow: Hỗ trợ mua sắm

KHÔNG tự xử lý - chỉ phân tích và chuyển đến workflow phù hợp.
"""
```

## 4. Deterministic Workflows với ADK

### 4.1 Product Workflow
```python
def create_product_workflow():
    """Product search and detail workflow"""
    
    # Step 1: Extract search parameters
    extract_params = FunctionWrapper(
        function=extract_search_params,
        name="extract_params",
        description="Extract search parameters from user input"
    )
    
    # Step 2: Search products via API
    search_products = FunctionWrapper(
        function=search_products_api,
        name="search_products",
        description="Search products using MM API"
    )
    
    # Step 3: Get recommendations
    get_recommendations = FunctionWrapper(
        function=get_cross_sell_products,
        name="get_recommendations",
        description="Get product recommendations"
    )
    
    # Step 4: Format results
    format_results = FunctionWrapper(
        function=format_product_results,
        name="format_results",
        description="Format results for display"
    )
    
    # Sequential workflow
    return SequentialAgent(
        name="product_workflow",
        description="Handle product search and details",
        sub_agents=[
            extract_params,
            search_products,
            get_recommendations,
            format_results
        ]
    )
```

### 4.2 Cart Workflow với Parallel Validation
```python
def create_cart_workflow():
    """Shopping cart management workflow"""
    
    # Parallel validation steps
    validation_agents = ParallelAgent(
        name="cart_validations",
        sub_agents=[
            FunctionWrapper(validate_product_stock),
            FunctionWrapper(validate_cart_limits),
            FunctionWrapper(check_user_session)
        ]
    )
    
    # Sequential cart operations
    cart_operations = SequentialAgent(
        name="cart_operations",
        sub_agents=[
            FunctionWrapper(ensure_cart_exists),
            FunctionWrapper(execute_cart_action),
            FunctionWrapper(calculate_totals),
            FunctionWrapper(format_cart_response)
        ]
    )
    
    # Main cart workflow
    return SequentialAgent(
        name="cart_workflow",
        description="Manage shopping cart operations",
        sub_agents=[
            FunctionWrapper(parse_cart_intent),
            validation_agents,
            cart_operations
        ]
    )
```

### 4.3 Order Workflow
```python
def create_order_workflow():
    """Order processing workflow"""
    
    # Parallel pre-order validations
    order_validations = ParallelAgent(
        name="order_validations",
        sub_agents=[
            FunctionWrapper(validate_cart_contents),
            FunctionWrapper(validate_shipping_address),
            FunctionWrapper(validate_payment_method)
        ]
    )
    
    # Sequential order processing
    order_processing = SequentialAgent(
        name="order_processing",
        sub_agents=[
            FunctionWrapper(create_order),
            FunctionWrapper(process_payment),
            FunctionWrapper(confirm_order),
            FunctionWrapper(send_notifications)
        ]
    )
    
    return SequentialAgent(
        name="order_workflow",
        description="Process orders and payments",
        sub_agents=[
            order_validations,
            order_processing,
            FunctionWrapper(format_order_confirmation)
        ]
    )
```

## 5. Session & State Management

### 5.1 Custom SessionService
```python
from google.adk.sessions import BaseSessionService
import redis
import json

class TieredSessionService(BaseSessionService):
    """Session service với multiple storage tiers"""
    
    def __init__(self, redis_client, db_client):
        super().__init__()
        self.redis = redis_client
        self.db = db_client
        
    async def save_session(self, session_id: str, session_data: Dict):
        """Save session với intelligent tiering"""
        # Hot data trong Redis (recent messages, cart state)
        hot_data = {
            'last_message': session_data.get('last_message'),
            'cart_id': session_data.get('cart_id'),
            'current_workflow': session_data.get('current_workflow')
        }
        
        await self.redis.setex(
            f"session:{session_id}:hot",
            3600,  # 1 hour TTL
            json.dumps(hot_data)
        )
        
        # Full session trong database
        await self.db.upsert_session(session_id, session_data)
    
    async def load_session(self, session_id: str) -> Dict:
        """Load session từ multiple tiers"""
        # Try hot storage first
        hot_data = await self.redis.get(f"session:{session_id}:hot")
        if hot_data:
            hot_data = json.loads(hot_data)
        
        # Get full session from database
        full_session = await self.db.get_session(session_id) or {}
        
        # Merge hot data
        if hot_data:
            full_session.update(hot_data)
            
        return full_session
```

### 5.2 Context Management
```python
class ContextManager:
    """Manage conversation context"""
    
    def __init__(self, session_service):
        self.session_service = session_service
        
    async def inject_context(self, agent_context):
        """Inject relevant context into agent execution"""
        session_id = agent_context.session_id
        session_data = await self.session_service.load_session(session_id)
        
        # Add user profile
        user_id = agent_context.user_id
        user_profile = await self.get_user_profile(user_id)
        agent_context.state['user_profile'] = user_profile
        
        # Add cart state
        cart_id = session_data.get('cart_id')
        if cart_id:
            cart_info = await self.get_cart_info(cart_id)
            agent_context.state['cart'] = cart_info
        
        # Add recent products viewed
        recent_products = session_data.get('recent_products', [])
        agent_context.state['recent_products'] = recent_products
```

## 6. Memory Tools Implementation

### 6.1 Memory Tools for Root Agent
```python
from google.adk.tools import tool

@tool
async def memorize(key: str, value: Any, ttl: int = 3600) -> Dict:
    """Store information in memory"""
    memory_key = f"memory:{context.session_id}:{key}"
    await redis_client.setex(memory_key, ttl, json.dumps(value))
    return {"success": True, "message": f"Stored {key}"}

@tool
async def get_memory(key: str) -> Dict:
    """Retrieve information from memory"""
    memory_key = f"memory:{context.session_id}:{key}"
    value = await redis_client.get(memory_key)
    
    if value:
        return {"success": True, "value": json.loads(value)}
    return {"success": False, "message": f"No memory found for {key}"}

@tool
async def analyze_context() -> Dict:
    """Analyze current conversation context"""
    session_data = context.state.get('session_data', {})
    
    return {
        "has_active_cart": bool(session_data.get('cart_id')),
        "recent_search": session_data.get('last_search'),
        "workflow_state": session_data.get('current_workflow'),
        "user_logged_in": bool(session_data.get('user_id'))
    }
```

## 7. API Integration Layer

### 7.1 MM API Client
```python
class MMApiClient:
    """GraphQL API client for MM system"""
    
    def __init__(self, base_url: str, cache: CacheService):
        self.base_url = base_url
        self.cache = cache
        self.session = aiohttp.ClientSession()
    
    @retry(max_attempts=3)
    @cache_result(ttl=300)
    async def search_products(self, query: str, filters: Dict = None) -> Dict:
        """Search products với caching"""
        query_str = """
        query SearchProducts($query: String!, $filters: ProductFilters) {
            products(search: $query, filters: $filters) {
                items {
                    sku
                    name
                    price
                    image
                    stock_status
                }
                total_count
            }
        }
        """
        
        variables = {"query": query, "filters": filters or {}}
        return await self._execute_query(query_str, variables)
    
    async def add_to_cart(self, cart_id: str, sku: str, quantity: int) -> Dict:
        """Add product to cart"""
        mutation = """
        mutation AddToCart($cartId: String!, $sku: String!, $qty: Int!) {
            addProductToCart(
                cartId: $cartId
                product: {sku: $sku, quantity: $qty}
            ) {
                cart {
                    items {
                        sku
                        quantity
                        price
                    }
                    total_quantity
                    total_price
                }
            }
        }
        """
        
        variables = {"cartId": cart_id, "sku": sku, "qty": quantity}
        return await self._execute_query(mutation, variables)
```

## 8. Function Wrappers for Workflows

### 8.1 Product Search Functions
```python
async def extract_search_params(context):
    """Extract search parameters from user message"""
    user_message = context.new_message.content
    
    # Simple pattern matching
    search_query = extract_product_query(user_message)
    filters = extract_filters(user_message)
    
    context.state['search_params'] = {
        'query': search_query,
        'filters': filters
    }
    return context.state['search_params']

async def search_products_api(context):
    """Search products via MM API"""
    params = context.state.get('search_params', {})
    api_client = context.services.api_client
    
    result = await api_client.search_products(
        query=params['query'],
        filters=params['filters']
    )
    
    context.state['search_results'] = result
    return result

async def format_product_results(context):
    """Format search results for display"""
    results = context.state.get('search_results', {})
    products = results.get('products', {}).get('items', [])
    
    if not products:
        message = "Không tìm thấy sản phẩm phù hợp."
    else:
        message = f"Tìm thấy {results['total_count']} sản phẩm:\n\n"
        for product in products[:5]:  # Show top 5
            message += f"• {product['name']}\n"
            message += f"  Giá: {product['price']:,}đ\n"
            message += f"  SKU: {product['sku']}\n\n"
    
    return create_response_event(message)
```

## 9. Event System

### 9.1 Custom Events
```python
from google.adk.events import Event, EventType

class ProductViewedEvent(Event):
    """Event when user views product"""
    event_type = EventType.CUSTOM
    product_sku: str
    product_name: str
    timestamp: datetime

class CartUpdatedEvent(Event):
    """Event when cart is updated"""
    event_type = EventType.CUSTOM
    cart_id: str
    action: str  # added, removed, updated
    items: List[Dict]

class OrderCreatedEvent(Event):
    """Event when order is created"""
    event_type = EventType.CUSTOM
    order_id: str
    total_amount: float
    status: str
```

### 9.2 Event Handlers
```python
@event_handler(ProductViewedEvent)
async def handle_product_viewed(event: ProductViewedEvent, context):
    """Track product views for recommendations"""
    # Update user's viewed products
    await update_user_activity(
        user_id=context.user_id,
        activity_type="product_view",
        product_sku=event.product_sku
    )
    
    # Update session
    session_data = context.state.get('session_data', {})
    recent_products = session_data.get('recent_products', [])
    recent_products.insert(0, event.product_sku)
    session_data['recent_products'] = recent_products[:10]  # Keep last 10
    
    await context.session_service.save_session(
        context.session_id,
        session_data
    )
```

## 10. Runner Configuration

### 10.1 Main Application Runner
```python
from google.adk.runners import Runner

# Initialize services
redis_client = redis.Redis(host='localhost', port=6379)
db_client = DatabaseClient(connection_string=DB_URL)
api_client = MMApiClient(base_url=API_BASE_URL, cache=cache_service)

# Session service
session_service = TieredSessionService(redis_client, db_client)

# Create agents
root_agent = RootAgent()
cng_agent = CnGAgent()

# Configure runner
runner = Runner(
    agent=root_agent,
    app_name="mm_ecommerce_chatbot",
    session_service=session_service,
    include_contents='recent:20',  # Only include last 20 messages
    stream_response=True,
    timeout=30  # 30 second timeout
)

# Add middleware
runner.add_middleware(SecurityMiddleware())
runner.add_middleware(MetricsMiddleware())
runner.add_middleware(ErrorHandlingMiddleware())

# Start the application
async def main():
    async for event in runner.run_async(
        user_id=user_id,
        session_id=session_id,
        message=user_message
    ):
        if event.is_final_response():
            return event.content
```

## 11. Monitoring & Logging

### 11.1 Metrics Collection
```python
from prometheus_client import Counter, Histogram, Gauge

# Define metrics
request_counter = Counter(
    'chatbot_requests_total',
    'Total number of requests',
    ['agent', 'workflow']
)

response_time = Histogram(
    'chatbot_response_seconds',
    'Response time in seconds',
    ['agent', 'workflow']
)

active_sessions = Gauge(
    'chatbot_active_sessions',
    'Number of active sessions'
)

class MetricsMiddleware:
    """Middleware for metrics collection"""
    
    async def process(self, request, next):
        start_time = time.time()
        
        try:
            response = await next(request)
            
            # Record metrics
            request_counter.labels(
                agent=request.agent_name,
                workflow=request.workflow_name
            ).inc()
            
            response_time.labels(
                agent=request.agent_name,
                workflow=request.workflow_name
            ).observe(time.time() - start_time)
            
            return response
            
        except Exception as e:
            error_counter.labels(
                agent=request.agent_name,
                error_type=type(e).__name__
            ).inc()
            raise
```

## 12. Deployment Configuration

### 12.1 Docker Compose
```yaml
version: '3.8'

services:
  chatbot:
    build: .
    environment:
      - GEMINI_API_KEY=${GEMINI_API_KEY}
      - REDIS_URL=redis://redis:6379
      - DATABASE_URL=postgresql://postgres:password@db:5432/chatbot
    depends_on:
      - redis
      - db
    ports:
      - "8000:8000"
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: '2'
          memory: 4G
        reservations:
          cpus: '1'
          memory: 2G

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=chatbot
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  redis-data:
  postgres-data:
```

### 12.2 Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mm-chatbot
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mm-chatbot
  template:
    metadata:
      labels:
        app: mm-chatbot
    spec:
      containers:
      - name: chatbot
        image: mm-chatbot:latest
        ports:
        - containerPort: 8000
        env:
        - name: GEMINI_API_KEY
          valueFrom:
            secretKeyRef:
              name: chatbot-secrets
              key: gemini-api-key
        resources:
          requests:
            cpu: "1"
            memory: "2Gi"
          limits:
            cpu: "2"
            memory: "4Gi"
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /ready
            port: 8000
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: mm-chatbot-service
spec:
  selector:
    app: mm-chatbot
  ports:
  - port: 80
    targetPort: 8000
  type: LoadBalancer
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: mm-chatbot-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: mm-chatbot
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

## 13. Performance Analysis

### 13.1 Response Time Breakdown
```
Total Response Time: 15-30s
├── Root Agent Intent Analysis: 1-2s
├── CnG Agent Routing: 0.5-1s
├── Workflow Execution: 10-25s
│   ├── API Calls: 8-20s (main bottleneck)
│   ├── Validation: 1-2s
│   └── Response Formatting: 0.5-1s
└── Network Latency: 1-2s
```

### 13.2 Optimization Strategies
- Cache frequent queries (5-min TTL)
- Preload common data
- Use connection pooling
- Implement request batching
- Progressive response streaming

## 14. Cost Analysis

### 14.1 LLM Usage
```
Per Conversation:
- Root Agent: 1 call (~500 tokens)
- CnG Agent: 1 call (~300 tokens)
- Total: ~800 tokens/conversation

Monthly Estimation (50 concurrent users):
- Daily conversations: 1,000
- Monthly tokens: 24M
- Cost: ~$300-400/month
```

### 14.2 Infrastructure
```
- Compute (3 instances): $300/month
- Redis (3GB): $100/month
- PostgreSQL (100GB): $200/month
- Load Balancer: $50/month
- Total: ~$650/month
```

## 15. Summary

Kiến trúc đơn giản hóa này:
- **2 LLM Agents**: Root và CnG
- **6 Deterministic Workflows**: Product, Cart, Order, Account, Promotion, Support
- **ADK Native**: Sử dụng tối đa ADK patterns
- **Performance**: 15-30s response time
- **Scalability**: Hỗ trợ 50+ concurrent users
- **Cost Effective**: ~$1000/month total cost

Hệ thống này cân bằng giữa simplicity và functionality, phù hợp cho B2C e-commerce với moderate traffic.
