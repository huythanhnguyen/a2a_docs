# Google Agent Development Kit (ADK) - Hướng dẫn chi tiết

## Mục lục

### Home
- [Giới thiệu về ADK](#giới-thiệu-về-adk)

### Get Started
- [Installation](#installation)
- [Quickstart](#quickstart)
- [Quickstart (streaming)](#quickstart-streaming)
- [Testing](#testing)
- [Sample agents](#sample-agents)
- [About ADK](#about-adk)

### Tutorials
- [Agent Team](#agent-team)
  - [Step 1: Your First Agent - Basic Weather Lookup](#step-1-your-first-agent---basic-weather-lookup)
  - [Step 2: Going Multi-Model with LiteLLM [Optional]](#step-2-going-multi-model-with-litellm-optional)
  - [Step 3: Building an Agent Team - Delegation for Greetings & Farewells](#step-3-building-an-agent-team---delegation-for-greetings--farewells)
  - [Step 4: Adding Memory and Personalization with Session State](#step-4-adding-memory-and-personalization-with-session-state)
  - [Step 5: Adding Safety - Input Guardrail with before_model_callback](#step-5-adding-safety---input-guardrail-with-before_model_callback)
  - [Step 6: Adding Safety - Tool Argument Guardrail (before_tool_callback)](#step-6-adding-safety---tool-argument-guardrail-before_tool_callback)
  - [Conclusion: Your Agent Team is Ready!](#conclusion-your-agent-team-is-ready)

### Agents
- [LLM agents](#llm-agents)
- [Workflow agents](#workflow-agents)
- [Sequential agents](#sequential-agents)
- [Loop agents](#loop-agents)
- [Parallel agents](#parallel-agents)
- [Custom agents](#custom-agents)
- [Multi-agent systems](#multi-agent-systems)

### Models
- [Models](#models)

### Tools
- [Function tools](#function-tools)
- [Built-in tools](#built-in-tools)
- [Third party tools](#third-party-tools)
- [Google Cloud tools](#google-cloud-tools)
- [MCP tools](#mcp-tools)
- [OpenAPI tools](#openapi-tools)

### Authentication
- [Authentication](#authentication)

### Running Agents
- [Running Agents](#running-agents)

### Runtime Config
- [Runtime Config](#runtime-config)

### Deploy
- [Agent Engine](#agent-engine)
- [Cloud Run](#cloud-run)
- [GKE](#gke)

### Sessions & Memory
- [Session](#session)
- [State](#state)
- [Memory](#memory)

### Callbacks
- [Types of callbacks](#types-of-callbacks)
- [Callback patterns](#callback-patterns)

### Artifacts
- [Artifacts](#artifacts)

### Events
- [Events](#events)

### Context
- [Context](#context)

### Evaluate
- [Evaluate](#evaluate)

### MCP
- [MCP](#mcp)

### Streaming
- [Streaming](#streaming)

### Safety and Security
- [Safety and Security](#safety-and-security)

### Agent2Agent (A2A) Protocol
- [Agent2Agent (A2A) Protocol](#agent2agent-a2a-protocol)

### Community Resources
- [Community Resources](#community-resources)

### Contributing Guide
- [Contributing Guide](#contributing-guide)

### API Reference
- [API Reference](#api-reference)

---

## Home - Giới thiệu về ADK

### 1.1. Agent Development Kit là gì?

Agent Development Kit (ADK) là một framework mã nguồn mở linh hoạt và module hóa để phát triển và triển khai các AI agent. ADK có thể sử dụng với các LLM phổ biến và các công cụ AI mã nguồn mở, được thiết kế với trọng tâm tích hợp chặt chẽ với hệ sinh thái Google và mô hình Gemini.

ADK giúp bạn dễ dàng bắt đầu với các agent đơn giản được hỗ trợ bởi mô hình Gemini và công cụ Google AI, đồng thời cung cấp khả năng kiểm soát và cấu trúc cần thiết cho các kiến trúc agent phức tạp hơn.

### 1.2. Tính năng chính

- **Flexible Orchestration (Điều phối linh hoạt)**: Định nghĩa workflow sử dụng workflow agents (Sequential, Parallel, Loop) cho pipeline dự đoán được, hoặc tận dụng LLM-driven dynamic routing (LlmAgent transfer) cho hành vi thích ứng.

- **Multi-Agent Architecture (Kiến trúc đa agent)**: Xây dựng ứng dụng module và có thể mở rộng bằng cách kết hợp nhiều agent chuyên biệt trong một hệ thống phân cấp.

- **Rich Tool Ecosystem (Hệ sinh thái công cụ phong phú)**: Trang bị cho agent các khả năng đa dạng: sử dụng công cụ có sẵn (Search, Code Exec), tạo custom functions, tích hợp thư viện bên thứ 3 (LangChain, CrewAI), hoặc thậm chí sử dụng agent khác như công cụ.

- **Deployment Ready (Sẵn sàng triển khai)**: Container hóa và triển khai agent của bạn ở bất cứ đâu – chạy local, mở rộng với Vertex AI Agent Engine, hoặc tích hợp vào cơ sở hạ tầng tùy chỉnh sử dụng Cloud Run hoặc Docker.

- **Built-in Evaluation (Đánh giá tích hợp)**: Đánh giá hiệu suất agent một cách có hệ thống bằng cách đánh giá cả chất lượng phản hồi cuối cùng và quá trình thực thi từng bước với các test case được xác định trước.

- **Building Responsible Agents (Xây dựng agent có trách nhiệm)**: Học cách xây dựng các agent mạnh mẽ và đáng tin cậy bằng cách triển khai các mẫu và thực tiễn tốt nhất về AI có trách nhiệm.

## 2. Cài đặt ADK

### 2.1. Tạo và kích hoạt môi trường ảo

Khuyến nghị tạo môi trường Python ảo sử dụng venv:

```bash
python -m venv .venv
```

Kích hoạt môi trường ảo:

```bash
# Mac / Linux
source .venv/bin/activate

# Windows CMD:
.venv\Scripts\activate.bat

# Windows PowerShell:
.venv\Scripts\Activate.ps1
```

### 2.2. Cài đặt ADK

```bash
pip install google-adk
```

(Tùy chọn) Xác minh cài đặt:

```bash
python -m google_adk.version
```

## 3. Bắt đầu với ADK

### 3.1. Quickstart - Tạo agent đầu tiên

#### Bước 1: Tạo và kích hoạt môi trường ảo

```bash
# Tạo môi trường ảo
python -m venv .venv

# Kích hoạt (mỗi terminal mới)
# macOS/Linux:
source .venv/bin/activate
# Windows CMD:
.venv\Scripts\activate.bat
# Windows PowerShell:
.venv\Scripts\Activate.ps1
```

#### Bước 2: Cài đặt ADK

```bash
pip install google-adk
```

#### Bước 3: Tạo cấu trúc thư mục cho agent

```bash
# Trên Windows, khuyến nghị tạo file bằng File Explorer hoặc IDE
mkdir multi_tool_agent/
touch multi_tool_agent/__init__.py
touch multi_tool_agent/agent.py
touch multi_tool_agent/.env
```

#### Bước 4: Tạo agent với nhiều công cụ

Tạo file `agent.py` với nội dung sau:

```python
import datetime
from zoneinfo import ZoneInfo
from google.adk.agents import Agent

def get_weather(city: str) -> dict:
    """Lấy báo cáo thời tiết hiện tại cho một thành phố.
    
    Returns:
        dict: Dictionary chứa thông tin thời tiết với key 'status' 
        ('success' hoặc 'error') và key 'report' với chi tiết thời tiết
    """
    if city.lower() == "new york":
        return {
            "status": "success", 
            "report": "Thời tiết tại New York nắng với nhiệt độ 25°C"
        }
    else:
        return {
            "status": "error", 
            "error_message": f"Không có thông tin thời tiết cho '{city}'"
        }

def get_current_time(city: str) -> dict:
    """Trả về giờ hiện tại tại một thành phố"""
    if city.lower() == "new york":
        tz_identifier = "America/New_York"
    else:
        return {
            "status": "error", 
            "error_message": f"Không có thông tin múi giờ cho {city}"
        }
    
    tz = ZoneInfo(tz_identifier)
    now = datetime.datetime.now(tz)
    return {
        "status": "success",
        "report": f"Giờ hiện tại tại {city} là {now.strftime('%Y-%m-%d %H:%M:%S %Z%z')}"
    }

# Tạo agent với các công cụ
root_agent = Agent(
    name="weather_time_agent",
    model="gemini-2.0-flash",
    description="Agent trả lời câu hỏi về thời gian và thời tiết",
    instruction="Tôi có thể trả lời câu hỏi về thời gian và thời tiết tại các thành phố",
    tools=[get_weather, get_current_time]
)
```

#### Bước 5: Cấu hình authentication

Tạo file `.env` với nội dung:

```bash
# Nếu sử dụng Google AI Studio
GOOGLE_API_KEY="your-api-key"
GOOGLE_GENAI_USE_VERTEXAI="FALSE"

# Nếu sử dụng Vertex AI trên Google Cloud
GOOGLE_CLOUD_PROJECT="your-project-id"
GOOGLE_CLOUD_LOCATION="us-central1"
GOOGLE_GENAI_USE_VERTEXAI="TRUE"
```

#### Bước 6: Chạy agent

Có nhiều cách để tương tác với agent:

1. **Development UI (adk web)**:
```bash
# Di chuyển đến thư mục cha
cd ..
# Chạy web UI
adk web multi_tool_agent/
```

2. **Terminal (adk run)**:
```bash
adk run multi_tool_agent/
```

3. **API Server (adk api_server)**:
```bash
adk api_server multi_tool_agent/
```

### 3.2. Tutorial - Tạo multi-agent system

#### Bước 1: Định nghĩa các agent chuyên biệt

```python
from google.adk.agents import LlmAgent

# Agent chào hỏi
greeter = LlmAgent(
    name="greeter",
    model="gemini-2.0-flash",
    instruction="""Bạn là agent chào đón. 
    Nhiệm vụ của bạn là chào hỏi người dùng một cách thân thiện
    và hỏi xem họ cần gì."""
)

# Agent thực thi tác vụ
task_executor = LlmAgent(
    name="task_executor",
    model="gemini-2.0-flash",
    instruction="""Bạn là agent thực thi tác vụ cụ thể.
    Khi được yêu cầu, hãy xử lý các tác vụ phức tạp.""",
    tools=[some_task_tool]  # Thêm các công cụ cần thiết
)
```

#### Bước 2: Tạo agent điều phối

```python
# Agent điều phối chính
coordinator = LlmAgent(
    name="coordinator",
    model="gemini-2.0-flash",
    description="Tôi điều phối các tác vụ giữa các agent",
    sub_agents=[greeter, task_executor],  # Gán các sub-agents
    instruction="""
    Bạn là agent điều phối chính. Nhiệm vụ của bạn là:
    1. Phân tích yêu cầu của người dùng
    2. Chuyển hướng đến agent phù hợp:
       - Sử dụng 'greeter' cho việc chào hỏi ban đầu
       - Sử dụng 'task_executor' cho các tác vụ cụ thể
    3. Tổng hợp kết quả và trả lời người dùng
    """
)
```

#### Bước 3: Cấu hình workflow nâng cao

```python
from google.adk.agents import SequentialAgent, ParallelAgent

# Sequential workflow - thực thi tuần tự
sequential_workflow = SequentialAgent(
    name="sequential_workflow",
    sub_agents=[
        data_collector,
        data_processor,
        report_generator
    ]
)

# Parallel workflow - thực thi song song
parallel_workflow = ParallelAgent(
    name="parallel_workflow",
    sub_agents=[
        search_agent_1,
        search_agent_2,
        search_agent_3
    ]
)
```

### 3.3. Quickstart với Streaming

ADK hỗ trợ streaming với voice và video cho giao tiếp real-time:

```python
from google.adk.agents import Agent
from google.adk.tools import google_search

# Agent với khả năng streaming
streaming_agent = Agent(
    name="streaming_assistant",
    model="gemini-2.0-flash",  # Model hỗ trợ Live API
    instruction="Tôi có thể trả lời câu hỏi và tìm kiếm thông tin real-time",
    tools=[google_search]
)

# Sử dụng với streaming UI
# adk web my_streaming_agent/ --streaming
```

#### Tạo ứng dụng FastAPI với WebSocket:

```python
from fastapi import FastAPI, WebSocket
from google.adk.runners import Runner
from google.adk.sessions import InMemorySessionService

app = FastAPI()
session_service = InMemorySessionService()

@app.websocket("/ws/{session_id}")
async def websocket_endpoint(websocket: WebSocket, session_id: str):
    await websocket.accept()
    
    runner = Runner(
        agent=streaming_agent,
        session_service=session_service
    )
    
    # Xử lý streaming bidirectional
    async for event in runner.run_async(
        session_id=session_id,
        streaming=True
    ):
        # Gửi response về client
        await websocket.send_json(event)
```

### 3.4. Các ví dụ mẫu

Google cung cấp repository với nhiều ví dụ agent mẫu:

```bash
# Clone repository mẫu
git clone https://github.com/google/adk-samples.git
cd adk-samples
```

Các loại agent mẫu:
- **Retail agents**: Hỗ trợ bán hàng, tư vấn sản phẩm
- **Travel agents**: Lập kế hoạch du lịch, đặt vé
- **Customer service agents**: Hỗ trợ khách hàng, xử lý khiếu nại
- **Data analysis agents**: Phân tích dữ liệu, tạo báo cáo
- **Code assistant agents**: Hỗ trợ lập trình, review code

## 4. Các thành phần chính

### 4.1. Loại Agent trong ADK

ADK cung cấp nhiều loại agent khác nhau để xây dựng các ứng dụng phức tạp:

#### 4.1.1. LLM Agents (LlmAgent, Agent)
Các agent này sử dụng Large Language Models (LLMs) làm engine chính để hiểu ngôn ngữ tự nhiên, lập luận, lên kế hoạch, tạo phản hồi và quyết định động cách thực hiện hoặc công cụ nào sẽ sử dụng.

```python
from google.adk.agents import LlmAgent, Agent  # Agent là alias của LlmAgent

# Ví dụ cơ bản về LLM Agent
capital_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="capital_agent",
    description="Trả lời câu hỏi về thủ đô của các quốc gia",
    instruction="""Bạn là một agent cung cấp thông tin về thủ đô của các quốc gia.
    Khi người dùng hỏi về thủ đô của một quốc gia:
    1. Xác định tên quốc gia từ câu hỏi
    2. Sử dụng công cụ `get_capital_city` để tìm thủ đô
    3. Trả lời rõ ràng cho người dùng
    """,
    tools=[get_capital_city]  # Danh sách các công cụ
)
```

#### 4.1.2. Workflow Agents
Các agent chuyên biệt này kiểm soát luồng thực thi của các agent khác theo mẫu xác định trước (tuần tự, song song, hoặc vòng lặp) mà không sử dụng LLM để kiểm soát luồng:

- `SequentialAgent`: Thực thi các agent con theo thứ tự
- `ParallelAgent`: Thực thi các agent con song song
- `LoopAgent`: Thực thi các agent con trong vòng lặp

#### 4.1.3. Custom Agents
Được tạo bằng cách mở rộng từ `BaseAgent`, cho phép bạn triển khai logic hoạt động độc đáo, luồng kiểm soát cụ thể hoặc tích hợp chuyên biệt.

### 4.2. Cấu hình LLM Agent chi tiết

#### 4.2.1. Định nghĩa danh tính và mục đích
Mỗi agent cần có:

```python
agent = LlmAgent(
    # Bắt buộc: Tên định danh duy nhất
    name="unique_agent_name",
    
    # Bắt buộc: Model LLM để sử dụng
    model="gemini-2.0-flash",
    
    # Khuyến nghị: Mô tả ngắn gọn về khả năng của agent
    description="Xử lý các câu hỏi về thanh toán và hóa đơn",
    
    # Instructions chi tiết (xem phần sau)
    instruction="...",
)
```

#### 4.2.2. Hướng dẫn Agent với Instructions

Instructions là tham số quan trọng nhất để định hình hành vi của LlmAgent:

```python
agent = LlmAgent(
    model="gemini-2.0-flash",
    name="customer_support",
    instruction="""
    Bạn là một trợ lý hỗ trợ khách hàng thân thiện.
    
    NHIỆM VỤ CHÍNH:
    - Trả lời câu hỏi về sản phẩm
    - Hỗ trợ xử lý đơn hàng
    - Giải quyết khiếu nại
    
    HƯỚNG DẪN SỬ DỤNG CÔNG CỤ:
    - Sử dụng `search_order` khi khách hàng hỏi về đơn hàng
    - Sử dụng `check_inventory` khi cần kiểm tra tồn kho
    
    ĐỊNH DẠNG TRẢ LỜI:
    - Luôn lịch sự và chuyên nghiệp
    - Cung cấp câu trả lời rõ ràng và chi tiết
    
    VÍ DỤ:
    Khách hàng: "Đơn hàng #12345 của tôi đang ở đâu?"
    Bạn: [Sử dụng search_order] "Tôi đã kiểm tra đơn hàng #12345..."
    """
)
```

#### 4.2.3. Trang bị công cụ cho Agent

```python
# Định nghĩa công cụ như hàm Python
def get_weather(city: str) -> dict:
    """Lấy thông tin thời tiết cho một thành phố."""
    # Logic lấy thời tiết
    return {"status": "success", "temperature": "25°C"}

# Hoặc tạo custom tool từ BaseTool
from google.adk.tools import BaseTool

class CustomSearchTool(BaseTool):
    name = "custom_search"
    description = "Tìm kiếm thông tin tùy chỉnh"
    
    def execute(self, query: str) -> str:
        # Logic tìm kiếm
        return f"Kết quả cho: {query}"

# Gán công cụ cho agent
agent = LlmAgent(
    model="gemini-2.0-flash",
    name="weather_agent",
    tools=[
        get_weather,  # Function tool
        CustomSearchTool(),  # Custom tool
        another_agent  # Hoặc một agent khác như công cụ
    ]
)
```

#### 4.2.4. Cấu hình nâng cao

```python
from google.genai.types import GenerateContentConfig
from pydantic import BaseModel, Field

# Cấu hình generation
config = GenerateContentConfig(
    temperature=0.7,  # Độ ngẫu nhiên (0.0 - 1.0)
    max_output_tokens=2000,  # Độ dài phản hồi tối đa
    top_p=0.9,
    top_k=40
)

# Schema cho input/output có cấu trúc
class QueryInput(BaseModel):
    query: str = Field(description="Câu hỏi của người dùng")
    context: str = Field(description="Ngữ cảnh bổ sung")

class AnswerOutput(BaseModel):
    answer: str = Field(description="Câu trả lời")
    confidence: float = Field(description="Độ tin cậy (0-1)")

# Agent với cấu hình nâng cao
advanced_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="advanced_agent",
    generate_content_config=config,
    input_schema=QueryInput,   # Ràng buộc input
    output_schema=AnswerOutput, # Ràng buộc output  
    output_key="result",       # Lưu kết quả vào state
    include_contents='none'    # Không nhận lịch sử hội thoại
)
```

### 4.3. Workflow Agents - Điều khiển luồng thực thi

ADK cung cấp các workflow agents để quản lý luồng thực thi của sub-agents một cách xác định:

#### 4.3.1. Sequential Agent

Thực thi các sub-agents theo thứ tự tuần tự:

```python
from google.adk.agents import SequentialAgent, LlmAgent

# Bước 1: Viết code
code_writer = LlmAgent(
    name="code_writer",
    model="gemini-2.0-flash",
    instruction="Viết code Python theo yêu cầu người dùng",
    output_key="draft_code"
)

# Bước 2: Review code
code_reviewer = LlmAgent(
    name="code_reviewer",
    model="gemini-2.0-flash",
    instruction="Review code từ {draft_code} và đưa ra nhận xét",
    output_key="review_comments"
)

# Bước 3: Refactor code
code_refactorer = LlmAgent(
    name="code_refactorer",
    model="gemini-2.0-flash",
    instruction="Cải thiện code dựa trên review comments: {review_comments}",
    output_key="final_code"
)

# Pipeline tuần tự
code_pipeline = SequentialAgent(
    name="code_pipeline",
    sub_agents=[code_writer, code_reviewer, code_refactorer],
    description="Pipeline viết, review và refactor code"
)
```

#### 4.3.2. Parallel Agent

Thực thi các sub-agents đồng thời:

```python
from google.adk.agents import ParallelAgent
from google.adk.tools import google_search

# Các agent nghiên cứu độc lập
renewable_researcher = LlmAgent(
    name="renewable_researcher",
    model="gemini-2.0-flash",
    instruction="Nghiên cứu về năng lượng tái tạo",
    tools=[google_search],
    output_key="renewable_findings"
)

ev_researcher = LlmAgent(
    name="ev_researcher",
    model="gemini-2.0-flash",
    instruction="Nghiên cứu về xe điện",
    tools=[google_search],
    output_key="ev_findings"
)

carbon_researcher = LlmAgent(
    name="carbon_researcher",
    model="gemini-2.0-flash",
    instruction="Nghiên cứu về công nghệ thu hồi carbon",
    tools=[google_search],
    output_key="carbon_findings"
)

# Thực thi song song
parallel_research = ParallelAgent(
    name="parallel_research",
    sub_agents=[renewable_researcher, ev_researcher, carbon_researcher],
    description="Nghiên cứu song song các chủ đề khác nhau"
)

# Tổng hợp kết quả
synthesizer = LlmAgent(
    name="synthesizer",
    model="gemini-2.0-flash",
    instruction="""
    Tổng hợp các kết quả nghiên cứu:
    - Năng lượng tái tạo: {renewable_findings}
    - Xe điện: {ev_findings}
    - Thu hồi carbon: {carbon_findings}
    """,
    output_key="research_summary"
)

# Pipeline hoàn chỉnh
research_pipeline = SequentialAgent(
    name="research_pipeline",
    sub_agents=[parallel_research, synthesizer],
    description="Nghiên cứu song song và tổng hợp kết quả"
)
```

#### 4.3.3. Loop Agent

Thực thi các sub-agents trong vòng lặp:

```python
from google.adk.agents import LoopAgent

# Agent viết nội dung
writer = LlmAgent(
    name="writer",
    model="gemini-2.0-flash",
    instruction="Viết hoặc cải thiện bài viết dựa trên phản hồi",
    output_key="current_draft"
)

# Agent đánh giá chất lượng
critic = LlmAgent(
    name="critic",
    model="gemini-2.0-flash",
    instruction="""
    Đánh giá chất lượng bài viết từ {current_draft}.
    Nếu chất lượng tốt, đặt state['should_continue'] = False.
    Nếu cần cải thiện, đưa ra góp ý chi tiết.
    """,
    output_key="critique"
)

# Vòng lặp cải thiện
refinement_loop = LoopAgent(
    name="refinement_loop",
    sub_agents=[writer, critic],
    max_iterations=5,  # Giới hạn số lần lặp
    description="Lặp lại việc viết và đánh giá cho đến khi đạt chất lượng"
)
```

### 4.4. Multi-Agent Systems - Hệ thống đa tác nhân

#### 4.3.1. Sử dụng Gemini Models

ADK được tối ưu hóa cho Gemini models:

```python
# Sử dụng Gemini qua Google AI Studio
import os
os.environ["GOOGLE_API_KEY"] = "YOUR_API_KEY"
os.environ["GOOGLE_GENAI_USE_VERTEXAI"] = "FALSE"

agent = LlmAgent(
    model="gemini-2.0-flash",  # Hoặc "gemini-2.5-pro-preview-03-25"
    name="gemini_agent"
)

# Sử dụng Gemini qua Vertex AI
os.environ["GOOGLE_CLOUD_PROJECT"] = "your-project-id"
os.environ["GOOGLE_CLOUD_LOCATION"] = "us-central1"
os.environ["GOOGLE_GENAI_USE_VERTEXAI"] = "TRUE"

agent = LlmAgent(
    model="gemini-2.0-flash",
    name="vertex_gemini_agent"
)
```

#### 4.3.2. Tích hợp các model khác qua LiteLLM

ADK hỗ trợ nhiều model khác thông qua LiteLLM:

```python
from google.adk.models.lite_llm import LiteLlm

# Sử dụng OpenAI GPT
os.environ["OPENAI_API_KEY"] = "your-openai-key"
gpt_agent = LlmAgent(
    name="gpt_agent",
    model=LiteLlm(model="openai/gpt-4o"),
    description="Agent sử dụng GPT-4"
)

# Sử dụng Anthropic Claude
os.environ["ANTHROPIC_API_KEY"] = "your-anthropic-key"
claude_agent = LlmAgent(
    name="claude_agent",
    model=LiteLlm(model="anthropic/claude-3-sonnet-20240229"),
    description="Agent sử dụng Claude"
)
```

### 4.4. Multi-Agent Systems

ADK cho phép xây dựng hệ thống đa agent phức tạp với nhiều cấp độ:

#### 4.4.1. Hierarchical Agent Systems

```python
# Định nghĩa các agent chuyên biệt
billing_agent = LlmAgent(
    name="billing_specialist",
    model="gemini-2.0-flash",
    instruction="Xử lý các vấn đề về thanh toán và hóa đơn...",
    tools=[check_invoice, process_payment]
)

shipping_agent = LlmAgent(
    name="shipping_specialist",
    model="gemini-2.0-flash",
    instruction="Xử lý vấn đề về vận chuyển và giao hàng...",
    tools=[track_shipment, update_delivery]
)

# Agent quản lý cấp trung 
customer_service = LlmAgent(
    name="customer_service",
    model="gemini-2.0-flash",
    sub_agents=[billing_agent, shipping_agent],
    instruction="""
    Bạn là agent dịch vụ khách hàng. Phân tích yêu cầu và chuyển đến:
    - billing_specialist: cho vấn đề thanh toán
    - shipping_specialist: cho vấn đề vận chuyển
    """
)

# Agent điều phối cấp cao nhất
main_coordinator = LlmAgent(
    name="main_coordinator",
    model="gemini-2.0-flash",
    sub_agents=[customer_service, sales_team, technical_support],
    global_instruction="""
    Đây là hướng dẫn chung áp dụng cho toàn bộ hệ thống:
    - Luôn lịch sự và chuyên nghiệp
    - Ưu tiên giải quyết vấn đề của khách hàng
    - Ghi lại mọi tương tác quan trọng
    """
)
```

#### 4.4.2. Agent Communication Patterns

```python
# Pattern 1: Direct Transfer
coordinator = LlmAgent(
    name="coordinator",
    instruction="""
    Khi cần xử lý tác vụ cụ thể, sử dụng cú pháp:
    <anythingllm:transfer>agent_name</anythingllm:transfer>
    để chuyển control đến agent phù hợp.
    """
)

# Pattern 2: Parallel Execution
from google.adk.agents import ParallelAgent

parallel_research = ParallelAgent(
    name="parallel_research",
    sub_agents=[
        web_search_agent,
        database_query_agent,
        document_analysis_agent
    ]
)

# Pattern 3: Sequential Pipeline
from google.adk.agents import SequentialAgent

data_pipeline = SequentialAgent(
    name="data_pipeline",
    sub_agents=[
        data_extractor,
        data_validator,
        data_transformer,
        data_loader
    ]
)
```

### 4.5. Tools - Công cụ cho Agent

#### 4.5.1. Built-in Tools

```python
from google.adk.tools import google_search, code_execution
from google.adk.tools.code_execution import CodeExecutionTool, SandboxedPythonExecutor

# Google Search
search_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="researcher",
    tools=[google_search],
    instruction="Tìm kiếm thông tin mới nhất về các chủ đề được hỏi"
)

# Code Execution
code_executor = CodeExecutionTool(
    executor=SandboxedPythonExecutor()
)

analysis_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="data_analyst",
    tools=[code_executor],
    instruction="Phân tích dữ liệu và tạo visualization bằng Python"
)
```

#### 4.5.2. Custom Tools

```python
from google.adk.tools import BaseTool
from typing import Dict, Any

# Tool đơn giản dạng function
def calculate_discount(price: float, discount_percent: float) -> float:
    """Tính giá sau khi giảm giá."""
    return price * (1 - discount_percent / 100)

# Tool phức tạp với BaseTool
class DatabaseTool(BaseTool):
    name = "database_query"
    description = "Truy vấn database để lấy thông tin"
    
    def __init__(self, connection_string: str):
        self.connection_string = connection_string
        
    def execute(self, query: str) -> Dict[str, Any]:
        # Logic kết nối và truy vấn database
        result = self._run_query(query)
        return {"status": "success", "data": result}
    
    def _run_query(self, query: str):
        # Implementation chi tiết
        pass

# Sử dụng các tools
sales_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="sales_assistant",
    tools=[
        calculate_discount,  # Function tool
        DatabaseTool("connection_string"),  # Custom tool
        inventory_agent  # Agent as tool
    ]
)
```

#### 4.5.3. Third-party Integration

```python
# Tích hợp với LangChain
from langchain.tools import WikipediaQueryRun
from langchain.utilities import WikipediaAPIWrapper

wikipedia = WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper())

research_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="wiki_researcher",
    tools=[wikipedia],
    instruction="Tìm kiếm thông tin trên Wikipedia khi cần thiết"
)
```

#### 4.5.4. Google Cloud Tools

ADK tích hợp sẵn với nhiều Google Cloud services:

```python
from google.adk.tools.gcp import (
    BigQueryTool,
    CloudStorageTool,
    CloudSQLTool,
    VertexAISearchTool
)

# BigQuery tool
bigquery_tool = BigQueryTool(
    project_id="your-project-id",
    dataset_id="your-dataset"
)

# Cloud Storage tool
storage_tool = CloudStorageTool(
    bucket_name="your-bucket",
    project_id="your-project-id"
)

# Vertex AI Search
search_tool = VertexAISearchTool(
    project_id="your-project-id",
    location="us-central1",
    search_engine_id="your-search-engine-id"
)

# Cloud SQL tool
sql_tool = CloudSQLTool(
    instance_connection_name="project:region:instance",
    database="your-database",
    user="your-user",
    password="your-password"
)

# Agent with Google Cloud tools
cloud_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="cloud_data_analyst",
    tools=[bigquery_tool, storage_tool, search_tool, sql_tool],
    instruction="""
    Analyze data across Google Cloud services:
    - Query data from BigQuery
    - Read/write files to Cloud Storage
    - Search documents with Vertex AI Search
    - Query Cloud SQL databases
    """
)
```

#### 4.5.5. MCP (Model Context Protocol) Tools

```python
from google.adk.tools.mcp import McpTool

# MCP weather tool
mcp_weather = McpTool(
    server_params={
        "command": "npx",
        "args": ["@modelcontextprotocol/server-weather"]
    }
)

# MCP filesystem tool
mcp_filesystem = McpTool(
    server_params={
        "command": "npx",
        "args": ["@modelcontextprotocol/server-filesystem", "--path", "/data"]
    }
)

# Agent with MCP tools
mcp_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="mcp_assistant",
    tools=[mcp_weather, mcp_filesystem],
    instruction="Use MCP tools to interact with weather data and filesystem"
)
```

#### 4.5.6. OpenAPI Tools

ADK hỗ trợ tạo tools từ OpenAPI specifications:

```python
from google.adk.tools.openapi import OpenAPITool

# Tạo tool từ OpenAPI spec
petstore_tool = OpenAPITool(
    spec_url="https://petstore.swagger.io/v2/swagger.json",
    api_key="your-api-key-if-needed"
)

# Hoặc từ local file
local_api_tool = OpenAPITool(
    spec_path="./api_spec.yaml",
    base_url="https://api.example.com",
    headers={"Authorization": "Bearer token"}
)

# Custom OpenAPI tool với specific operations
github_tool = OpenAPITool(
    spec_url="https://raw.githubusercontent.com/github/rest-api-description/main/descriptions/api.github.com/api.github.com.json",
    operations=["repos/get", "users/get", "search/repositories"],
    headers={"Authorization": f"token {GITHUB_TOKEN}"}
)

# Agent with OpenAPI tools
api_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="api_integrator",
    tools=[petstore_tool, github_tool],
    instruction="""
    Interact with external APIs:
    - Use petstore API for pet-related queries
    - Use GitHub API for repository information
    Follow the OpenAPI specifications for correct usage.
    """
)
```

## 5. Triển khai và Production

### 5.1. Deployment Options

ADK hỗ trợ nhiều cách triển khai:

#### 5.1.1. Local Development

```bash
# Development UI
adk web my_agent/

# CLI testing
adk run my_agent/

# API server
adk api_server my_agent/
```

#### 5.1.2. Container Deployment

```dockerfile
# Dockerfile
FROM python:3.10-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

#### 5.1.3. Vertex AI Deployment

```python
from vertexai.preview import reasoning_engines

# Deploy to Vertex AI
remote_app = reasoning_engines.ReasoningEngine.create(
    display_name="My ADK Agent",
    agent=my_agent,
    requirements=[
        "google-adk",
        "google-cloud-aiplatform"
    ]
)

# Sử dụng agent đã deploy
response = remote_app.query(
    input="What's the weather in Tokyo?",
    config={"session_id": "123"}
)
```

#### 5.1.4. Google Kubernetes Engine (GKE) Deployment

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adk-agent-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: adk-agent
  template:
    metadata:
      labels:
        app: adk-agent
    spec:
      containers:
      - name: adk-agent
        image: gcr.io/your-project/adk-agent:latest
        ports:
        - containerPort: 8000
        env:
        - name: GOOGLE_CLOUD_PROJECT
          value: "your-project-id"
        - name: GOOGLE_APPLICATION_CREDENTIALS
          value: "/secrets/service-account.json"
        volumeMounts:
        - name: service-account
          mountPath: "/secrets"
          readOnly: true
      volumes:
      - name: service-account
        secret:
          secretName: adk-service-account
---
apiVersion: v1
kind: Service
metadata:
  name: adk-agent-service
spec:
  selector:
    app: adk-agent
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8000
  type: LoadBalancer
```

```bash
# Build and push Docker image
docker build -t gcr.io/your-project/adk-agent:latest .
docker push gcr.io/your-project/adk-agent:latest

# Deploy to GKE
kubectl apply -f deployment.yaml

# Get external IP
kubectl get service adk-agent-service

# Scale deployment
kubectl scale deployment adk-agent-deployment --replicas=5

# Update deployment
kubectl set image deployment/adk-agent-deployment \
  adk-agent=gcr.io/your-project/adk-agent:v2

# Monitor pods
kubectl get pods -l app=adk-agent
kubectl logs -f deployment/adk-agent-deployment
```

Configuration cho Horizontal Pod Autoscaler:

```yaml
# hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: adk-agent-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: adk-agent-deployment
  minReplicas: 2
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

### 5.2. Testing và Evaluation

ADK cung cấp framework để test và đánh giá agent:

#### 5.2.1. Unit Testing

```python
import unittest
from google.adk.agents import LlmAgent

class TestMyAgent(unittest.TestCase):
    def setUp(self):
        self.agent = LlmAgent(
            name="test_agent",
            model="gemini-2.0-flash",
            instruction="You are a helpful assistant."
        )
    
    def test_agent_response(self):
        # Test logic here
        pass
```

#### 5.2.2. Evaluation Framework

```json
// evaluation_set.json
[
  {
    "input": "What's the capital of France?",
    "expected_output": "Paris",
    "expected_tools": ["get_capital_city"],
    "metrics": ["accuracy", "relevance"]
  },
  {
    "input": "Calculate 15% discount on $100",
    "expected_output": "$85",
    "expected_tools": ["calculate_discount"]
  }
]
```

```bash
# Chạy evaluation
adk eval my_agent/ evaluation_set.json
```

#### 5.2.3. Testing with CLI

```bash
# Test agent via command line
adk run my_agent/

# Test với specific input
adk run my_agent/ --input "What's the weather in Tokyo?"
```

### 5.3. Sessions và Memory

ADK hỗ trợ quản lý state và memory qua sessions:

```python
from google.adk.sessions import InMemorySessionService
from google.adk.runners import Runner

# Tạo session service
session_service = InMemorySessionService()

# Tạo session mới
session_service.create_session(
    app_name="my_app",
    user_id="user_123",
    session_id="session_456"
)

# Sử dụng với Runner
runner = Runner(
    agent=my_agent,
    app_name="my_app",
    session_service=session_service
)

# State được duy trì giữa các lời gọi
async for event in runner.run_async(
    user_id="user_123",
    session_id="session_456",
    new_message="Remember my name is John"
):
    print(event)
```

### 5.4. Safety và Security

#### 5.4.1. Input Validation với Callbacks

```python
from google.adk.agents import LlmAgent

def before_model_callback(ctx):
    """Validate input trước khi gửi đến model"""
    user_input = ctx.message.content
    
    # Kiểm tra nội dung không phù hợp
    if has_inappropriate_content(user_input):
        return Event(
            author="safety_guard",
            content=types.Content(
                role="system",
                parts=[types.Part(text="Nội dung không phù hợp.")]
            )
        )
    
    return None  # Cho phép tiếp tục

agent = LlmAgent(
    model="gemini-2.0-flash",
    name="safe_agent",
    before_model_callback=before_model_callback
)
```

#### 5.4.2. Tool Safety

```python
def before_tool_callback(ctx):
    """Validate tool arguments"""
    tool_name = ctx.function_call.name
    args = ctx.function_call.args
    
    # Kiểm tra arguments
    if tool_name == "database_query":
        if has_sql_injection(args.get("query")):
            raise ValueError("Potential SQL injection detected")
    
    return None

agent = LlmAgent(
    model="gemini-2.0-flash",
    name="secure_agent",
    before_tool_callback=before_tool_callback
)
```

### 5.5. Agent2Agent (A2A) Protocol

ADK hỗ trợ giao thức A2A cho giao tiếp giữa các agent:

```python
# Expose agent qua A2A endpoint
from fastapi import FastAPI
from google.adk.a2a import create_a2a_endpoint

app = FastAPI()

# Tạo A2A endpoint
a2a_endpoint = create_a2a_endpoint(agent=my_agent)
app.mount("/", a2a_endpoint)

# Metadata cho A2A discovery
@app.get("/.well-known/agent.json")
async def agent_metadata():
    return {
        "name": "my_agent",
        "description": "A helpful assistant",
        "version": "1.0.0",
        "capabilities": ["text", "tools"]
    }
```

## 6. Advanced Topics

### 6.1. Custom Agents

Tạo agent tùy chỉnh bằng cách extend BaseAgent:

```python
from google.adk.agents import BaseAgent
from google.adk.events import Event, EventActions
from google.genai import types

class CustomAnalyzerAgent(BaseAgent):
    """Custom agent với logic phân tích đặc biệt"""
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.analysis_threshold = 0.8
    
    async def _run_async_impl(self, context):
        # Custom logic here
        user_message = context.new_message
        
        # Phân tích sentiment
        sentiment_score = self.analyze_sentiment(user_message)
        
        if sentiment_score < self.analysis_threshold:
            # Chuyển hướng đến agent khác
            yield Event(
                author=self.name,
                actions=EventActions(
                    transfer_to="support_specialist"
                )
            )
        else:
            # Xử lý bình thường
            response = self.generate_response(user_message)
            yield Event(
                author=self.name,
                content=types.Content(
                    role="assistant",
                    parts=[types.Part(text=response)]
                )
            )
    
    def analyze_sentiment(self, message):
        # Logic phân tích sentiment
        return 0.9
    
    def generate_response(self, message):
        # Logic tạo phản hồi
        return "Tôi hiểu vấn đề của bạn..."
```

### 6.2. Authentication

Cấu hình authentication cho các services:

```python
# Google Cloud authentication
import os
os.environ["GOOGLE_CLOUD_PROJECT"] = "your-project-id"
os.environ["GOOGLE_CLOUD_LOCATION"] = "us-central1"
os.environ["GOOGLE_GENAI_USE_VERTEXAI"] = "TRUE"

# API keys cho third-party services
os.environ["OPENAI_API_KEY"] = "your-openai-key"
os.environ["ANTHROPIC_API_KEY"] = "your-anthropic-key"

# OAuth cho Google services
from google.oauth2 import service_account
credentials = service_account.Credentials.from_service_account_file(
    'path/to/service-account.json'
)
```

### 6.3. MCP (Model Context Protocol) Tools

Tích hợp MCP tools vào agent:

```python
from google.adk.tools.mcp import McpTool

# Tạo MCP tool
mcp_weather = McpTool(
    server_params={
        "command": "npx",
        "args": ["@modelcontextprotocol/server-weather", "--location", "US"]
    }
)

# Sử dụng trong agent
weather_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="weather_assistant",
    tools=[mcp_weather],
    instruction="Use MCP weather tool to provide weather forecasts"
)
```

### 6.4. Runtime Configuration

Cấu hình runtime cho agent:

```python
from google.adk.runtime import RuntimeConfig

runtime_config = RuntimeConfig(
    timeout=30,  # Timeout in seconds
    retry_attempts=3,
    retry_delay=1.0,
    max_concurrent_tools=5,
    enable_streaming=True
)

agent = LlmAgent(
    model="gemini-2.0-flash",
    name="configured_agent",
    runtime_config=runtime_config
)
```

### 6.5. Callbacks và Hooks

Sử dụng callbacks để kiểm soát execution flow:

```python
from google.adk.callbacks import CallbackHandler

class CustomCallbackHandler(CallbackHandler):
    async def on_agent_start(self, agent, context):
        print(f"Agent {agent.name} started")
    
    async def on_agent_end(self, agent, context, result):
        print(f"Agent {agent.name} finished")
    
    async def on_tool_start(self, tool, args):
        print(f"Tool {tool.name} called with {args}")
    
    async def on_tool_end(self, tool, result):
        print(f"Tool {tool.name} returned {result}")
    
    async def on_error(self, error):
        print(f"Error occurred: {error}")

# Sử dụng callback handler
agent = LlmAgent(
    model="gemini-2.0-flash",
    name="monitored_agent",
    callback_handlers=[CustomCallbackHandler()]
)
```

## 7. Tutorials - Hướng dẫn từng bước

### 7.1. Agent Team Tutorial

Xây dựng một team agent từ cơ bản đến nâng cao:

#### Step 1: Agent đầu tiên - Basic Weather Lookup

```python
from google.adk.agents import Agent
from google.adk.tools import google_search

# Agent cơ bản với công cụ tìm kiếm
weather_agent = Agent(
    name="weather_assistant",
    model="gemini-2.0-flash",
    instruction="Help users find weather information using search",
    tools=[google_search]
)
```

#### Step 2: Tích hợp nhiều models với LiteLLM

```python
from google.adk.models.lite_llm import LiteLlm

# Sử dụng GPT-4 cho complex reasoning
complex_agent = Agent(
    name="complex_reasoner",
    model=LiteLlm(model="openai/gpt-4"),
    instruction="Handle complex analytical tasks"
)

# Sử dụng Claude cho creative writing
creative_agent = Agent(
    name="creative_writer",
    model=LiteLlm(model="anthropic/claude-3-sonnet"),
    instruction="Generate creative content"
)
```

#### Step 3: Xây dựng Agent Team

```python
# Agents chuyên biệt
greeter = Agent(
    name="greeter",
    model="gemini-2.0-flash",
    instruction="Greet users warmly and understand their needs"
)

farewell = Agent(
    name="farewell",
    model="gemini-2.0-flash",
    instruction="Provide pleasant farewells and follow-ups"
)

# Coordinator agent
team_coordinator = Agent(
    name="coordinator",
    model="gemini-2.0-flash",
    sub_agents=[greeter, farewell, weather_agent],
    instruction="""
    Coordinate the team:
    - Use 'greeter' for initial interactions
    - Use 'weather_assistant' for weather queries
    - Use 'farewell' for ending conversations
    """
)
```

#### Step 4: Thêm Memory và Personalization

```python
team_coordinator = Agent(
    name="coordinator_with_memory",
    model="gemini-2.0-flash",
    sub_agents=[greeter, farewell, weather_agent],
    instruction="""
    Remember user preferences in state:
    - state['user_name']: User's name
    - state['preferences']: User preferences
    
    Personalize interactions based on stored information.
    """,
    output_key="conversation_summary"
)
```

#### Step 5: Safety với before_model_callback

```python
def input_safety_guard(context):
    """Check for inappropriate content before processing"""
    user_input = context.new_message.content
    
    if contains_inappropriate_content(user_input):
        return Event(
            author="safety_guard",
            content=types.Content(
                role="system",
                parts=[types.Part(text="I cannot process inappropriate content.")]
            )
        )
    
    return None

safe_agent = Agent(
    name="safe_coordinator",
    model="gemini-2.0-flash",
    before_model_callback=input_safety_guard,
    sub_agents=[greeter, farewell, weather_agent]
)
```

#### Step 6: Tool Safety với before_tool_callback

```python
def tool_safety_check(context):
    """Validate tool arguments before execution"""
    tool_name = context.function_call.name
    args = context.function_call.args
    
    # Kiểm tra SQL injection
    if tool_name == "database_query":
        if has_sql_injection(args.get("query")):
            raise ValueError("SQL injection detected")
    
    # Giới hạn API calls
    if tool_name == "external_api":
        if context.session.state.get("api_calls", 0) > 100:
            raise ValueError("API rate limit exceeded")
    
    return None

secure_agent = Agent(
    name="secure_coordinator",
    model="gemini-2.0-flash",
    before_tool_callback=tool_safety_check,
    tools=[database_query, external_api]
)
```

## 8. Complete Examples

### 8.1. Multi-Agent Customer Service System

```python
from google.adk.agents import LlmAgent, SequentialAgent, ParallelAgent
from google.adk.tools import google_search
from google.adk.sessions import InMemorySessionService
from google.adk.runners import Runner

# Billing specialist
billing_agent = LlmAgent(
    name="billing_specialist",
    model="gemini-2.0-flash",
    instruction="""
    You are a billing specialist. Handle questions about:
    - Invoices and payments
    - Subscription plans
    - Refunds and credits
    """,
    tools=[check_invoice, process_payment],
    output_key="billing_response"
)

# Technical support
tech_support = LlmAgent(
    name="tech_support",
    model="gemini-2.0-flash",
    instruction="""
    You are a technical support specialist. Help with:
    - Troubleshooting issues
    - Configuration problems
    - Bug reports
    """,
    tools=[check_system_status, create_ticket],
    output_key="tech_response"
)

# Sales specialist
sales_agent = LlmAgent(
    name="sales_specialist",
    model="gemini-2.0-flash",
    instruction="""
    You are a sales specialist. Handle:
    - Product information
    - Pricing inquiries
    - Demo requests
    """,
    tools=[get_product_info, schedule_demo],
    output_key="sales_response"
)

# Parallel investigation
parallel_investigation = ParallelAgent(
    name="parallel_investigation",
    sub_agents=[billing_agent, tech_support, sales_agent]
)

# Solution synthesizer
synthesizer = LlmAgent(
    name="solution_synthesizer",
    model="gemini-2.0-flash",
    instruction="""
    Analyze results from specialists:
    - Billing: {billing_response}
    - Technical: {tech_response}
    - Sales: {sales_response}
    
    Provide a comprehensive solution to the customer.
    """,
    output_key="final_solution"
)

# Complete customer service pipeline
customer_service = SequentialAgent(
    name="customer_service_system",
    sub_agents=[parallel_investigation, synthesizer]
)

# Main coordinator
main_agent = LlmAgent(
    name="customer_service_coordinator",
    model="gemini-2.0-flash",
    sub_agents=[customer_service],
    instruction="""
    You are the main customer service coordinator.
    1. Understand the customer's issue
    2. Route to appropriate specialists
    3. Ensure customer satisfaction
    """,
    global_instruction="""
    Always be polite and professional.
    Prioritize customer satisfaction.
    Document all interactions.
    """
)

# Setup and run
async def main():
    session_service = InMemorySessionService()
    runner = Runner(
        agent=main_agent,
        app_name="customer_service",
        session_service=session_service
    )
    
    # Create session
    session_service.create_session(
        app_name="customer_service",
        user_id="customer_123",
        session_id="session_456"
    )
    
    # Process customer request
    async for event in runner.run_async(
        user_id="customer_123",
        session_id="session_456",
        new_message="I'm having trouble with my billing and the app is crashing"
    ):
        if event.is_final_response():
            print(f"Response: {event.content.parts[0].text}")
```

### 8.2. Research Assistant with Dynamic Parallelism

```python
from google.adk.agents import BaseAgent, ParallelAgent, LlmAgent
from google.adk.events import Event, EventActions
import secrets

class DynamicResearchCoordinator(BaseAgent):
    """Dynamically creates parallel research tasks based on query"""
    
    async def _run_async_impl(self, context):
        query = context.new_message.content
        
        # Analyze query to determine research areas
        areas = self.identify_research_areas(query)
        
        # Dynamically create researcher agents
        researchers = []
        run_id = secrets.token_hex(4)
        
        for i, area in enumerate(areas):
            researcher = LlmAgent(
                name=f"researcher_{i}_{run_id}",
                model="gemini-2.0-flash",
                instruction=f"Research about: {area}",
                tools=[google_search],
                output_key=f"research_{area}_{run_id}"
            )
            researchers.append(researcher)
        
        # Create dynamic parallel agent
        parallel_research = ParallelAgent(
            name=f"parallel_research_{run_id}",
            sub_agents=researchers
        )
        
        # Execute parallel research
        async for event in parallel_research.run_async(context):
            yield event
        
        # Synthesize results
        synthesis_agent = LlmAgent(
            name="synthesizer",
            model="gemini-2.0-flash",
            instruction=self.build_synthesis_instruction(areas, run_id)
        )
        
        async for event in synthesis_agent.run_async(context):
            yield event
    
    def identify_research_areas(self, query):
        # Logic to identify research areas from query
        # This could use an LLM or rule-based approach
        return ["topic1", "topic2", "topic3"]
    
    def build_synthesis_instruction(self, areas, run_id):
        instruction = "Synthesize the following research results:\n"
        for area in areas:
            instruction += f"- {area}: {{research_{area}_{run_id}}}\n"
        return instruction
```

Tài liệu này bây giờ đã bao gồm hầu hết các phần quan trọng của Google ADK. Các phần đã được cập nhật:

1. ✅ Get Started & Installation
2. ✅ Quickstart & Quickstart (streaming)
3. ✅ Testing & Evaluation
4. ✅ Agents (LLM, Workflow, Custom)
5. ✅ Multi-agent systems
6. ✅ Models
7. ✅ Tools (Function, Built-in, Third-party)
8. ✅ Authentication
9. ✅ Deployment (Vertex AI, Cloud Run, Container)
10. ✅ Sessions & Memory
11. ✅ Callbacks
12. ✅ Safety and Security
13. ✅ Agent2Agent Protocol
14. ✅ Tutorials (Agent Team steps)
15. ✅ Complete Examples

Tài liệu này giờ đã khá đầy đủ và chi tiết về Google ADK!
