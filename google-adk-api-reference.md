# Google ADK API Reference - Chi tiết các Module

## Mục lục

### Submodules
- [google.adk.agents module](#googleadkagents-module)
- [google.adk.artifacts module](#googleadkartifacts-module)
- [google.adk.code_executors module](#googleadkcode_executors-module)
- [google.adk.evaluation module](#googleadkevaluation-module)
- [google.adk.events module](#googleadkevents-module)
- [google.adk.examples module](#googleadkexamples-module)
- [google.adk.memory module](#googleadkmemory-module)
- [google.adk.models module](#googleadkmodels-module)
- [google.adk.planners module](#googleadkplanners-module)
- [google.adk.runners module](#googleadkrunners-module)
- [google.adk.sessions module](#googleadksessions-module)
- [google.adk.tools module](#googleadktools-module)

---

## google.adk.agents module

Module chứa các class để định nghĩa và quản lý agents trong ADK.

### Classes

#### BaseAgent
Base class cho tất cả agents trong ADK.

```python
class BaseAgent:
    """Base class for all agents in Agent Development Kit"""
    
    # Properties
    name: str                    # Tên của agent (bắt buộc)
    description: str = ""        # Mô tả về khả năng của agent
    parent_agent: Optional[BaseAgent] = None     # Agent cha
    sub_agents: Optional[List[BaseAgent]] = None # Danh sách agent con
    before_agent_callback: Optional[Callable] = None
    after_agent_callback: Optional[Callable] = None
    
    # Methods
    def find_agent(self, name: str) -> Optional[BaseAgent]:
        """Tìm agent với tên cho trước trong agent này và con cháu"""
        
    def find_descendant_agent(self, name: str) -> Optional[BaseAgent]:
        """Tìm agent với tên cho trước trong các agent con"""
        
    def get_root_agent(self) -> BaseAgent:
        """Lấy root agent của agent này"""
        
    async def run_async(self, context) -> AsyncIterator[Event]:
        """Entry method để chạy agent qua text-based conversation"""
        
    async def run_live(self, context) -> AsyncIterator[Event]:
        """Entry method để chạy agent qua video/audio-based conversation"""
```

#### Agent (LlmAgent)
Agent được hỗ trợ bởi Large Language Model.

```python
class LlmAgent(BaseAgent):
    """LLM-powered agent"""
    
    # Properties
    model: str                   # Model LLM sử dụng
    instruction: str             # Instruction cho agent
    tools: List[Tool] = []       # Danh sách tools
    generate_content_config: Optional[GenerateContentConfig] = None
    input_schema: Optional[BaseModel] = None   # Schema cho input
    output_schema: Optional[BaseModel] = None  # Schema cho output
    output_key: Optional[str] = None          # Key để lưu output vào state
    include_contents: str = 'default'         # Kiểm soát context history
    planner: Optional[BasePlanner] = None     # Planner cho multi-step reasoning
    code_executor: Optional[BaseCodeExecutor] = None
    examples: List[Example] = []              # Few-shot examples
    global_instruction: Optional[str] = None  # Global instruction cho sub-agents
    
    # Transfer control properties
    disallow_transfer_to_parent: bool = False
    disallow_transfer_to_peers: bool = False
    
    # Callbacks
    before_model_callback: Optional[Callable] = None
    after_model_callback: Optional[Callable] = None
    before_tool_callback: Optional[Callable] = None
    after_tool_callback: Optional[Callable] = None
    
    # Methods
    def canonical_instruction(self) -> str:
        """Lấy instruction đã được xử lý"""
        
    def canonical_global_instruction(self) -> str:
        """Lấy global instruction đã được xử lý"""
        
    @property
    def canonical_model(self) -> BaseLlm:
        """Lấy model instance"""
        
    @property
    def canonical_tools(self) -> List[Tool]:
        """Lấy danh sách tools bao gồm cả agent tools"""
```

#### SequentialAgent
Agent thực thi sub-agents theo thứ tự tuần tự.

```python
class SequentialAgent(BaseAgent):
    """Sequential workflow agent"""
    
    # Thực thi các sub_agents theo thứ tự được định nghĩa
    async def run_async(self, context) -> AsyncIterator[Event]:
        """Chạy từng sub-agent theo thứ tự"""
```

#### ParallelAgent
Agent thực thi sub-agents đồng thời.

```python
class ParallelAgent(BaseAgent):
    """Parallel workflow agent"""
    
    # Thực thi các sub_agents song song
    async def run_async(self, context) -> AsyncIterator[Event]:
        """Chạy tất cả sub-agents đồng thời"""
```

#### LoopAgent
Agent thực thi sub-agents trong vòng lặp.

```python
class LoopAgent(BaseAgent):
    """Loop workflow agent"""
    
    max_iterations: int = 5      # Số lần lặp tối đa
    
    async def run_async(self, context) -> AsyncIterator[Event]:
        """Chạy sub-agents trong vòng lặp"""
```

---

## google.adk.artifacts module

Module quản lý artifacts (files, data) được tạo và sử dụng bởi agents.

### Classes

#### BaseArtifactService
Abstract base class cho artifact services.

```python
class BaseArtifactService(ABC):
    """Base class for artifact management"""
    
    @abstractmethod
    async def save(self, artifact_id: str, content: bytes) -> None:
        """Lưu artifact"""
        
    @abstractmethod
    async def load(self, artifact_id: str) -> bytes:
        """Load artifact"""
        
    @abstractmethod
    async def exists(self, artifact_id: str) -> bool:
        """Kiểm tra artifact có tồn tại không"""
        
    @abstractmethod
    async def delete(self, artifact_id: str) -> None:
        """Xóa artifact"""
```

#### InMemoryArtifactService
Artifact service lưu trữ trong memory.

```python
class InMemoryArtifactService(BaseArtifactService):
    """In-memory artifact storage"""
    
    # Implementation lưu artifacts trong dictionary
    artifacts: Dict[str, bytes] = {}
```

#### GcsArtifactService
Artifact service sử dụng Google Cloud Storage.

```python
class GcsArtifactService(BaseArtifactService):
    """Google Cloud Storage artifact service"""
    
    bucket_name: str             # GCS bucket name
    credentials: Optional[Credentials] = None
```

---

## google.adk.code_executors module

Module để thực thi code trong agents.

### Classes

#### BaseCodeExecutor
Base class cho code executors.

```python
class BaseCodeExecutor(ABC):
    """Base code executor"""
    
    stateful: bool = False       # Có duy trì state giữa các executions
    error_retry_attempts: int = 0
    optimize_data_file: bool = True
    code_block_delimiters: Tuple[str, str] = ('```', '```')
    execution_result_delimiters: Tuple[str, str] = ('<result>', '</result>')
    
    @abstractmethod
    async def execute_code(self, 
                          code: str, 
                          context: CodeExecutorContext) -> str:
        """Thực thi code và trả về kết quả"""
```

#### CodeExecutorContext
Context object cho code execution.

```python
class CodeExecutorContext:
    """Context for code execution"""
    
    # Methods để quản lý state và files
    def add_input_files(self, files: Dict[str, bytes]) -> None:
        """Thêm input files"""
        
    def get_input_files(self) -> Dict[str, bytes]:
        """Lấy input files"""
        
    def update_code_execution_result(self, result: str) -> None:
        """Cập nhật kết quả execution"""
        
    def get_state_delta(self) -> Dict[str, Any]:
        """Lấy state changes"""
```

#### VertexAiCodeExecutor
Code executor sử dụng Vertex AI.

```python
class VertexAiCodeExecutor(BaseCodeExecutor):
    """Vertex AI code executor"""
    
    project_id: str
    location: str
    service_account: Optional[str] = None
```

#### ContainerCodeExecutor
Code executor sử dụng Docker container.

```python
class ContainerCodeExecutor(BaseCodeExecutor):
    """Docker container code executor"""
    
    image: str                   # Docker image
    base_url: str               # Container service URL
    docker_path: str = "docker"
```

#### UnsafeLocalCodeExecutor
Code executor chạy code locally (không an toàn).

```python
class UnsafeLocalCodeExecutor(BaseCodeExecutor):
    """Local code executor (unsafe)"""
    
    # Chạy code trực tiếp trên local machine
    # CHÚ Ý: Không an toàn cho production
```

---

## google.adk.evaluation module

Module để đánh giá performance của agents.

### Classes

#### AgentEvaluator
Class để evaluate agents.

```python
class AgentEvaluator:
    """Agent evaluation utilities"""
    
    @classmethod
    def evaluate(cls, 
                agent: BaseAgent,
                test_cases: List[Dict],
                metrics: List[str] = None) -> Dict:
        """Đánh giá agent với test cases"""
        
    @classmethod
    def find_config_for_test_file(cls, test_file: str) -> Dict:
        """Tìm cấu hình cho test file"""
```

---

## google.adk.events module

Module định nghĩa events được generate bởi agents.

### Classes

#### Event
Event object được tạo trong quá trình agent execution.

```python
class Event:
    """Event generated during agent execution"""
    
    # Properties
    id: str                      # Event ID
    timestamp: datetime          # Thời điểm tạo event
    author: str                  # Tác giả của event (agent name)
    content: Optional[Content] = None  # Nội dung event
    actions: Optional[EventActions] = None
    branch: str = ""            # Branch identifier
    invocation_id: Optional[str] = None
    long_running_tool_ids: List[str] = []
    
    # Methods
    def is_final_response(self) -> bool:
        """Kiểm tra có phải final response không"""
        
    def get_function_calls(self) -> List[FunctionCall]:
        """Lấy function calls từ event"""
        
    def get_function_responses(self) -> List[FunctionResponse]:
        """Lấy function responses từ event"""
        
    def has_trailing_code_execution_result(self) -> bool:
        """Kiểm tra có code execution result không"""
        
    @staticmethod
    def new_id() -> str:
        """Tạo ID mới cho event"""
```

#### EventActions
Actions có thể được thực hiện trong event.

```python
class EventActions:
    """Actions that can be taken in an event"""
    
    state_delta: Dict[str, Any] = {}      # Thay đổi state
    artifacts_delta: Dict[str, Any] = {}  # Thay đổi artifacts
    transfer_to: Optional[str] = None     # Transfer control đến agent khác
    escalate: bool = False               # Escalate to parent agent
```

---

## google.adk.examples module

Module chứa các ví dụ và sample code.

```python
# Module này chứa các ví dụ về cách sử dụng ADK
# Bao gồm sample agents, tools, và workflows
```

---

## google.adk.memory module

Module quản lý memory và persistent storage cho agents.

### Classes

#### BaseMemoryService
Abstract base class cho memory services.

```python
class BaseMemoryService(ABC):
    """Base memory service"""
    
    @abstractmethod
    async def store(self, key: str, value: Any) -> None:
        """Lưu trữ data vào memory"""
        
    @abstractmethod
    async def retrieve(self, key: str) -> Any:
        """Lấy data từ memory"""
        
    @abstractmethod
    async def search(self, query: str) -> List[Dict]:
        """Tìm kiếm trong memory"""
```

#### InMemoryMemoryService
Memory service sử dụng in-memory storage.

```python
class InMemoryMemoryService(BaseMemoryService):
    """In-memory storage service"""
    
    # Implementation với dictionary
    memory_store: Dict[str, Any] = {}
```

#### VertexAiRagMemoryService
Memory service sử dụng Vertex AI RAG.

```python
class VertexAiRagMemoryService(BaseMemoryService):
    """Vertex AI RAG memory service"""
    
    project_id: str
    location: str
    corpus_id: str
```

---

## google.adk.models module

Module định nghĩa các LLM models.

### Classes

#### BaseLlm
Abstract base class cho LLM integrations.

```python
class BaseLlm(ABC):
    """Base LLM interface"""
    
    @abstractmethod
    async def generate_content(self,
                             messages: List[Message],
                             config: Optional[GenerateContentConfig] = None) -> Content:
        """Generate content từ messages"""
```

#### Gemini
Gemini model implementation.

```python
class Gemini(BaseLlm):
    """Gemini model wrapper"""
    
    model_name: str              # Tên model (e.g., "gemini-2.0-flash")
    credentials: Optional[Credentials] = None
    use_vertexai: bool = False
```

#### LLMRegistry
Registry để quản lý các LLM models.

```python
class LLMRegistry:
    """Registry for LLM models"""
    
    @classmethod
    def register(cls, name: str, model_class: Type[BaseLlm]) -> None:
        """Đăng ký model mới"""
        
    @classmethod
    def get(cls, name: str) -> Type[BaseLlm]:
        """Lấy model class theo tên"""
```

---

## google.adk.planners module

Module cho planning và reasoning strategies.

### Classes

#### BasePlanner
Abstract base class cho planners.

```python
class BasePlanner(ABC):
    """Base planner interface"""
    
    @abstractmethod
    def build_planning_instruction(self, context) -> str:
        """Tạo planning instruction"""
        
    @abstractmethod
    def process_planning_response(self, response: str) -> Dict:
        """Xử lý planning response"""
```

#### BuiltInPlanner
Planner sử dụng built-in planning capabilities.

```python
class BuiltInPlanner(BasePlanner):
    """Built-in planner"""
    
    thinking_config: Optional[ThinkingConfig] = None
    
    def apply_thinking_config(self) -> None:
        """Áp dụng thinking configuration"""
```

#### PlanReActPlanner
Planner sử dụng Plan & React pattern.

```python
class PlanReActPlanner(BasePlanner):
    """Plan and React planner"""
    
    # Implementation của planning với reasoning và acting steps
```

---

## google.adk.runners module

Module để chạy agents.

### Classes

#### Runner
Main runner class để execute agents.

```python
class Runner:
    """Agent runner"""
    
    # Properties
    agent: BaseAgent             # Agent để chạy
    app_name: str               # Application name
    session_service: BaseSessionService
    memory_service: Optional[BaseMemoryService] = None
    artifact_service: Optional[BaseArtifactService] = None
    
    # Methods
    async def run_async(self,
                       user_id: str,
                       session_id: str,
                       new_message: Content) -> AsyncIterator[Event]:
        """Chạy agent với message mới"""
        
    def run(self, user_id: str, session_id: str, message: str) -> Iterator[Event]:
        """Synchronous wrapper cho run_async"""
        
    async def run_live(self,
                      user_id: str,
                      session_id: str,
                      audio_stream: AsyncIterator[bytes]) -> AsyncIterator[Event]:
        """Chạy agent với audio/video stream"""
        
    async def close_session(self, user_id: str, session_id: str) -> None:
        """Đóng session"""
```

#### InMemoryRunner
Runner với in-memory storage.

```python
class InMemoryRunner(Runner):
    """Runner with in-memory storage"""
    
    # Sử dụng InMemorySessionService mặc định
```

---

## google.adk.sessions module

Module quản lý sessions và state.

### Classes

#### Session
Session object chứa conversation state.

```python
class Session:
    """Conversation session"""
    
    session_id: str
    user_id: str
    app_name: str
    state: State                 # Session state
    events: List[Event] = []    # Event history
    created_at: datetime
    updated_at: datetime
```

#### State
State object để lưu trữ session data.

```python
class State(Dict[str, Any]):
    """Session state storage"""
    
    # Dictionary để lưu trữ state variables
    # Có thể truy cập như: state['key'] hoặc state.key
```

#### BaseSessionService
Abstract base class cho session services.

```python
class BaseSessionService(ABC):
    """Base session service"""
    
    @abstractmethod
    async def create_session(self,
                           app_name: str,
                           user_id: str,
                           session_id: str) -> Session:
        """Tạo session mới"""
        
    @abstractmethod
    async def get_session(self,
                         app_name: str,
                         user_id: str,
                         session_id: str) -> Session:
        """Lấy session"""
        
    @abstractmethod
    async def list_sessions(self,
                          app_name: str,
                          user_id: str) -> List[Session]:
        """Liệt kê sessions"""
        
    @abstractmethod
    async def delete_session(self,
                           app_name: str,
                           user_id: str,
                           session_id: str) -> None:
        """Xóa session"""
        
    @abstractmethod
    async def append_event(self,
                          app_name: str,
                          user_id: str,
                          session_id: str,
                          event: Event) -> None:
        """Thêm event vào session history"""
```

#### InMemorySessionService
Session service với in-memory storage.

```python
class InMemorySessionService(BaseSessionService):
    """In-memory session storage"""
    
    # Implementation với dictionary
    sessions: Dict[Tuple[str, str, str], Session] = {}
```

#### VertexAiSessionService
Session service sử dụng Vertex AI.

```python
class VertexAiSessionService(BaseSessionService):
    """Vertex AI session service"""
    
    project_id: str
    location: str
```

#### DatabaseSessionService
Session service sử dụng database.

```python
class DatabaseSessionService(BaseSessionService):
    """Database session service"""
    
    connection_string: str
    # Implementation với SQL database
```

---

## google.adk.tools module

Module định nghĩa tools cho agents.

### Classes

#### BaseTool
Abstract base class cho tools.

```python
class BaseTool(ABC):
    """Base tool interface"""
    
    name: str                    # Tên tool
    description: str            # Mô tả tool
    
    @abstractmethod
    async def execute(self, **kwargs) -> Any:
        """Thực thi tool với arguments"""
```

#### FunctionTool
Tool được tạo từ Python function.

```python
class FunctionTool(BaseTool):
    """Function-based tool"""
    
    function: Callable          # Python function
    
    @classmethod
    def from_function(cls, func: Callable) -> FunctionTool:
        """Tạo tool từ function"""
```

#### LongRunningFunctionTool
Tool cho các operations chạy lâu.

```python
class LongRunningFunctionTool(FunctionTool):
    """Long-running function tool"""
    
    # Hỗ trợ async operations và progress tracking
```

#### ExampleTool
Tool với example usage.

```python
class ExampleTool(BaseTool):
    """Tool with usage examples"""
    
    examples: List[Dict[str, Any]] = []
```

#### ToolContext
Context cho tool execution.

```python
class ToolContext:
    """Context for tool execution"""
    
    session: Session
    agent: BaseAgent
    event: Event
```

#### AuthToolArguments
Arguments cho authentication trong tools.

```python
class AuthToolArguments:
    """Authentication arguments for tools"""
    
    oauth_tokens: Dict[str, str] = {}
    api_keys: Dict[str, str] = {}
```

#### APIHubToolset
Toolset từ API Hub.

```python
class APIHubToolset:
    """API Hub toolset"""
    
    project_id: str
    location: str
    api_hub_id: str
    
    def get_tools(self) -> List[BaseTool]:
        """Lấy danh sách tools từ API Hub"""
```

#### VertexAiSearchTool
Tool cho Vertex AI Search.

```python
class VertexAiSearchTool(BaseTool):
    """Vertex AI Search tool"""
    
    project_id: str
    location: str
    search_engine_id: str
```

### Functions

#### exit_loop()
Function để thoát khỏi LoopAgent.

```python
def exit_loop() -> None:
    """Signal to exit from a loop agent"""
```

#### transfer_to_agent()
Function để chuyển control sang agent khác.

```python
def transfer_to_agent(agent_name: str) -> None:
    """Transfer control to another agent"""
```

---

## Tổng kết

API Reference của Google ADK cung cấp một hệ thống module hoàn chỉnh để xây dựng AI agents:

1. **Agents Module**: Định nghĩa các loại agents (LLM, Sequential, Parallel, Loop)
2. **Tools Module**: Tạo và quản lý tools cho agents
3. **Sessions Module**: Quản lý conversation state và history
4. **Runners Module**: Execute agents
5. **Memory Module**: Persistent storage
6. **Code Executors Module**: Execute code safely
7. **Evaluation Module**: Đánh giá agent performance
8. **Events Module**: Event system cho agent communication
9. **Models Module**: LLM integrations
10. **Planners Module**: Planning và reasoning strategies
11. **Artifacts Module**: File và data management
12. **Examples Module**: Sample code và tutorials

Mỗi module được thiết kế modular và có thể mở rộng, cho phép developers xây dựng các hệ thống agent phức tạp một cách linh hoạt.
