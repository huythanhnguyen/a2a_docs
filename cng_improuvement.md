<anythingllm identifier="mm-a2a-memory-browser" type="application/vnd.ant.code" language="python" title="MM A2A Memory Architecture with Browser Storage">
MM A2A Memory Architecture với Browser Storage (localStorage + IndexedDB)
1. Phân tích vấn đề hiện tại
Vấn đề cốt lõi trong Root Agent
python# Hiện tại - Root agent KHÔNG CÓ TOOLS
root_agent = Agent(
    model="gemini-2.0-flash-001",
    name="root_agent",
    description="Root Agent cho MM A2A Ecommerce Chatbot",
    instruction=prompt.ROOT_AGENT_INSTR,
    sub_agents=[cng_agent],
    # THIẾU: tools=[memorize, get_memory]  <- ĐÂY LÀ VẤN ĐỀ!
)
Các vấn đề liên quan

No Memory Tools: Root agent không có khả năng gọi memorize/get_memory
Callbacks chưa hook: ensure_event_loop và cleanup_resources đã định nghĩa nhưng chưa gắn vào agent
Memory Store đơn giản: Sử dụng dictionary in-memory không phù hợp production
Không có storage management: Thiếu cơ chế quản lý storage limits

2. Kiến trúc Memory mới với Browser Storage
2.1 Fixed Root Agent với Memory Tools
pythonfrom google.adk.agents import Agent
from mm_a2a.tools.memory import memorize, get_memory, forget

# Root Agent với đầy đủ memory capabilities
root_agent = Agent(
    model="gemini-2.0-flash-001",
    name="root_agent", 
    description="Root Agent cho MM A2A Ecommerce Chatbot",
    instruction=prompt.ROOT_AGENT_INSTR,
    sub_agents=[cng_agent],
    tools=[
        memorize,           # Ghi nhớ thông tin
        get_memory,         # Truy xuất thông tin
        forget,             # Xóa thông tin cũ
        memorize_list,      # Ghi nhớ danh sách
    ],
    # Callbacks để quản lý lifecycle
    on_before_run=ensure_event_loop,
    on_after_run=cleanup_resources,
)
2.2 Tiered Storage Architecture với Browser Storage
┌─────────────────────────────────────────────────────┐
│                   Tier 1: Hot Memory                │
│          (In-memory - Active Conversation)          │
│              • Current session state                │
│              • Recent 100 messages                  │
│              • Active cart/order                    │
│              • Size: Max 50MB per session          │
└─────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────┐
│               Tier 2: Warm Storage                  │
│        (localStorage - Recent 7 days)               │
│              • Recent conversations                 │
│              • User preferences                     │
│              • Order history                        │
│              • Size: Max 5MB (browser limit)       │
└─────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────┐
│               Tier 3: Cold Storage                  │
│     (IndexedDB - 30 days + Summarized)             │
│              • Compressed conversations             │
│              • Summarized context                   │
│              • Key decisions/facts                  │
│              • Size: Limited by device storage     │
└─────────────────────────────────────────────────────┘
2.3 Enhanced Memory Tools với Browser Storage Management
pythonfrom datetime import datetime, timedelta
from typing import Dict, Any, Optional
import json
import asyncio

# Browser Storage Interfaces (using through JavaScript interop)
class BrowserStorageInterface:
    """Interface to browser storage through JavaScript"""
    
    @staticmethod
    async def local_storage_set(key: str, value: Any):
        """Set value in localStorage"""
        # This would call JavaScript through WebAssembly or similar
        # In practice, you'd use a bridge to JavaScript
        js_code = f"""
        localStorage.setItem('{key}', JSON.stringify({json.dumps(value)}));
        """
        await execute_js(js_code)
    
    @staticmethod
    async def local_storage_get(key: str) -> Optional[Any]:
        """Get value from localStorage"""
        js_code = f"""
        return localStorage.getItem('{key}');
        """
        result = await execute_js(js_code)
        return json.loads(result) if result else None
    
    @staticmethod
    async def indexed_db_put(store_name: str, key: str, value: Any):
        """Store value in IndexedDB"""
        js_code = f"""
        const request = indexedDB.open('MM_A2A_DB', 1);
        request.onsuccess = function(event) {{
            const db = event.target.result;
            const transaction = db.transaction(['{store_name}'], 'readwrite');
            const store = transaction.objectStore('{store_name}');
            store.put({{key: '{key}', value: {json.dumps(value)}}});
        }};
        """
        await execute_js(js_code)
    
    @staticmethod
    async def indexed_db_get(store_name: str, key: str) -> Optional[Any]:
        """Get value from IndexedDB"""
        js_code = f"""
        return new Promise((resolve) => {{
            const request = indexedDB.open('MM_A2A_DB', 1);
            request.onsuccess = function(event) {{
                const db = event.target.result;
                const transaction = db.transaction(['{store_name}'], 'readonly');
                const store = transaction.objectStore('{store_name}');
                const getRequest = store.get('{key}');
                getRequest.onsuccess = function() {{
                    resolve(getRequest.result ? getRequest.result.value : null);
                }};
            }};
        }});
        """
        result = await execute_js(js_code)
        return result

class BrowserBasedMemory:
    """Memory system với browser storage limits và tiered architecture"""
    
    def __init__(self, max_memory_size=50*1024*1024):
        self.hot_memory = {}  # In-memory cache
        self.max_memory_size = max_memory_size
        self.storage = BrowserStorageInterface()
        
    async def memorize(self, key: str, value: Any, context: Dict[str, Any]) -> Dict[str, Any]:
        """Ghi nhớ với auto-tiering"""
        session_id = context.get('session_id', 'default')
        timestamp = datetime.utcnow()
        
        # Kiểm tra kích thước
        value_str = json.dumps(value)
        value_size = len(value_str.encode('utf-8'))
        
        # Tier 1: Hot memory cho dữ liệu nhỏ và mới
        if value_size < 1024 * 10:  # < 10KB
            self.hot_memory[f"{session_id}:{key}"] = {
                'value': value,
                'timestamp': timestamp,
                'size': value_size
            }
            
        # Tier 2: localStorage cho dữ liệu trung bình
        elif value_size < 1024 * 100:  # < 100KB
            storage_key = f"mm_a2a:{session_id}:{key}"
            storage_value = {
                'value': value,
                'timestamp': timestamp.isoformat(),
                'expires': (timestamp + timedelta(days=7)).isoformat()
            }
            
            try:
                await self.storage.local_storage_set(storage_key, storage_value)
            except Exception as e:
                # Handle localStorage quota exceeded
                await self._cleanup_local_storage()
                await self.storage.local_storage_set(storage_key, storage_value)
                
        # Tier 3: IndexedDB với compression cho dữ liệu lớn
        else:
            # Compress và lưu vào IndexedDB
            compressed_value = await self._compress_data(value)
            await self.storage.indexed_db_put(
                'conversations',
                f"{session_id}:{key}",
                {
                    'value': compressed_value,
                    'timestamp': timestamp.isoformat(),
                    'compressed': True
                }
            )
            
        # Trigger cleanup nếu vượt quota
        await self._check_and_cleanup(session_id)
        
        return {
            'success': True,
            'message': f'Đã lưu {key} với size {value_size} bytes',
            'tier': self._get_storage_tier(value_size)
        }
    
    async def get_memory(self, key: str, context: Dict[str, Any]) -> Dict[str, Any]:
        """Truy xuất từ multiple tiers"""
        session_id = context.get('session_id', 'default')
        
        # Check Tier 1: Hot memory
        hot_key = f"{session_id}:{key}"
        if hot_key in self.hot_memory:
            return {
                'success': True,
                'value': self.hot_memory[hot_key]['value'],
                'tier': 'hot'
            }
            
        # Check Tier 2: localStorage
        storage_key = f"mm_a2a:{session_id}:{key}"
        local_data = await self.storage.local_storage_get(storage_key)
        if local_data:
            # Check expiration
            expires = datetime.fromisoformat(local_data['expires'])
            if datetime.utcnow() < expires:
                return {
                    'success': True,
                    'value': local_data['value'],
                    'tier': 'warm'
                }
                
        # Check Tier 3: IndexedDB
        db_value = await self.storage.indexed_db_get('conversations', f"{session_id}:{key}")
        if db_value:
            value = db_value['value']
            if db_value.get('compressed'):
                value = await self._decompress_data(value)
            return {
                'success': True,
                'value': value,
                'tier': 'cold'
            }
            
        return {
            'success': False,
            'message': f'Không tìm thấy dữ liệu cho key {key}'
        }
    
    async def _check_and_cleanup(self, session_id: str):
        """Kiểm tra storage limits và cleanup"""
        # Check localStorage quota
        local_storage_usage = await self._get_local_storage_usage()
        if local_storage_usage > 4 * 1024 * 1024:  # 4MB threshold
            await self._cleanup_local_storage()
            
        # Check hot memory size
        hot_memory_size = self._calculate_hot_memory_size()
        if hot_memory_size > self.max_memory_size:
            await self._evict_from_hot_memory()
            
    async def _cleanup_local_storage(self):
        """Clean up localStorage by moving old data to IndexedDB"""
        all_keys = await self._get_all_local_storage_keys()
        items_with_timestamp = []
        
        for key in all_keys:
            if key.startswith('mm_a2a:'):
                value = await self.storage.local_storage_get(key)
                if value and 'timestamp' in value:
                    items_with_timestamp.append((key, value, value['timestamp']))
        
        # Sort by timestamp, oldest first
        items_with_timestamp.sort(key=lambda x: x[2])
        
        # Move oldest 50% to IndexedDB
        items_to_move = items_with_timestamp[:len(items_with_timestamp)//2]
        
        for key, value, _ in items_to_move:
            # Extract session_id and original key
            parts = key.split(':')
            if len(parts) >= 3:
                session_id = parts[1]
                original_key = ':'.join(parts[2:])
                
                # Move to IndexedDB
                await self.storage.indexed_db_put(
                    'conversations',
                    f"{session_id}:{original_key}",
                    value
                )
                
                # Remove from localStorage
                await self.storage.local_storage_remove(key)
    
    async def _compress_data(self, data: Any) -> str:
        """Compress data using browser-compatible compression"""
        # In browser, use CompressionStream API
        js_code = f"""
        const input = new TextEncoder().encode(JSON.stringify({json.dumps(data)}));
        const cs = new CompressionStream('gzip');
        const writer = cs.writable.getWriter();
        writer.write(input);
        writer.close();
        
        const compressed = [];
        const reader = cs.readable.getReader();
        let done = false;
        while (!done) {{
            const {{value, done: doneReading}} = await reader.read();
            done = doneReading;
            if (value) compressed.push(...value);
        }}
        
        return btoa(String.fromCharCode.apply(null, compressed));
        """
        return await execute_js(js_code)
    
    async def _decompress_data(self, compressed: str) -> Any:
        """Decompress data using browser-compatible decompression"""
        js_code = f"""
        const compressed = atob('{compressed}');
        const bytes = new Uint8Array(compressed.split('').map(char => char.charCodeAt(0)));
        
        const ds = new DecompressionStream('gzip');
        const writer = ds.writable.getWriter();
        writer.write(bytes);
        writer.close();
        
        const decompressed = [];
        const reader = ds.readable.getReader();
        let done = false;
        while (!done) {{
            const {{value, done: doneReading}} = await reader.read();
            done = doneReading;
            if (value) decompressed.push(...value);
        }}
        
        const text = new TextDecoder().decode(new Uint8Array(decompressed));
        return JSON.parse(text);
        """
        return await execute_js(js_code)
2.4 Summarization Agent cho Context Compression
pythonfrom google.adk.agents import Agent

# Agent chuyên dùng để tóm tắt và nén context
summarizer_agent = Agent(
    model="gemini-2.0-flash-001",
    name="summarizer_agent",
    description="Agent tóm tắt và nén context để tiết kiệm storage",
    instruction="""
    Bạn là agent chuyên tóm tắt hội thoại và nén context.
    
    Nhiệm vụ:
    1. Tóm tắt cuộc hội thoại thành các key points
    2. Giữ lại thông tin quan trọng: orders, preferences, decisions
    3. Bỏ qua chi tiết không cần thiết
    4. Output dạng JSON structure để dễ query
    
    Format output:
    {
        "summary": "Tóm tắt ngắn gọn",
        "key_facts": ["fact1", "fact2"],
        "user_preferences": {...},
        "important_events": [...],
        "timestamp": "ISO date"
    }
    """,
    output_schema={
        "type": "object",
        "properties": {
            "summary": {"type": "string"},
            "key_facts": {"type": "array", "items": {"type": "string"}},
            "user_preferences": {"type": "object"},
            "important_events": {"type": "array"},
            "timestamp": {"type": "string"}
        }
    }
)
2.5 Browser-Based Background Tasks
pythonimport asyncio
from datetime import datetime, timedelta

class BrowserMemoryScheduler:
    """Background tasks for browser-based memory system"""
    
    def __init__(self, memory_system: BrowserBasedMemory):
        self.memory = memory_system
        self.running = False
        
    async def start(self):
        """Start background maintenance using browser APIs"""
        self.running = True
        
        # Use browser's Background Sync API for periodic tasks
        js_code = """
        // Register background sync
        if ('serviceWorker' in navigator && 'SyncManager' in window) {
            navigator.serviceWorker.ready.then(function(registration) {
                registration.periodicSync.register('memory-cleanup', {
                    minInterval: 60 * 60 * 1000  // 1 hour
                });
            });
        }
        
        // Also use requestIdleCallback for non-critical tasks
        function scheduleCleanup() {
            requestIdleCallback(() => {
                window.postMessage({type: 'MEMORY_CLEANUP'}, '*');
                setTimeout(scheduleCleanup, 3600000);  // Schedule next cleanup
            });
        }
        scheduleCleanup();
        """
        await execute_js(js_code)
        
        # Listen for cleanup messages
        await self._setup_message_listener()
    
    async def _setup_message_listener(self):
        """Setup listener for cleanup messages"""
        while self.running:
            # This would be implemented through JavaScript event listeners
            message = await wait_for_js_message()
            
            if message.get('type') == 'MEMORY_CLEANUP':
                await self._perform_cleanup()
            elif message.get('type') == 'SUMMARIZE_OLD_DATA':
                await self._summarize_old_conversations()
                
            await asyncio.sleep(0.1)  # Prevent tight loop
    
    async def _perform_cleanup(self):
        """Perform memory cleanup"""
        try:
            # Clean up hot memory
            await self.memory._evict_from_hot_memory()
            
            # Clean up localStorage
            await self.memory._cleanup_local_storage()
            
            # Clean up old IndexedDB entries
            await self._cleanup_old_indexed_db_entries()
            
        except Exception as e:
            logger.error(f"Cleanup error: {e}")
    
    async def _cleanup_old_indexed_db_entries(self):
        """Remove old entries from IndexedDB"""
        js_code = """
        const request = indexedDB.open('MM_A2A_DB', 1);
        request.onsuccess = function(event) {
            const db = event.target.result;
            const transaction = db.transaction(['conversations'], 'readwrite');
            const store = transaction.objectStore('conversations');
            
            // Get all entries
            const getAllRequest = store.getAll();
            getAllRequest.onsuccess = function() {
                const allEntries = getAllRequest.result;
                const now = new Date();
                const thirtyDaysAgo = new Date(now - 30 * 24 * 60 * 60 * 1000);
                
                // Delete entries older than 30 days
                allEntries.forEach(entry => {
                    if (entry.value.timestamp) {
                        const entryDate = new Date(entry.value.timestamp);
                        if (entryDate < thirtyDaysAgo) {
                            store.delete(entry.key);
                        }
                    }
                });
            };
        };
        """
        await execute_js(js_code)
    
    async def _summarize_old_conversations(self):
        """Summarize old conversations using the summarizer agent"""
        # Get conversations older than 7 days from IndexedDB
        js_code = """
        return new Promise((resolve) => {
            const request = indexedDB.open('MM_A2A_DB', 1);
            request.onsuccess = function(event) {
                const db = event.target.result;
                const transaction = db.transaction(['conversations'], 'readonly');
                const store = transaction.objectStore('conversations');
                
                const getAllRequest = store.getAll();
                getAllRequest.onsuccess = function() {
                    const allEntries = getAllRequest.result;
                    const now = new Date();
                    const sevenDaysAgo = new Date(now - 7 * 24 * 60 * 60 * 1000);
                    
                    const oldConversations = allEntries.filter(entry => {
                        if (entry.value.timestamp) {
                            const entryDate = new Date(entry.value.timestamp);
                            return entryDate < sevenDaysAgo && !entry.value.summarized;
                        }
                        return false;
                    });
                    
                    resolve(oldConversations);
                };
            };
        });
        """
        
        old_conversations = await execute_js(js_code)
        
        for conv in old_conversations:
            # Extract messages and summarize
            messages = conv['value']['value']
            
            # Use summarizer agent
            summary = await summarizer_agent.run(
                input_data={'messages': messages}
            )
            
            # Store summary back to IndexedDB
            await self.memory.storage.indexed_db_put(
                'conversations',
                conv['key'],
                {
                    'value': summary,
                    'timestamp': conv['value']['timestamp'],
                    'summarized': True,
                    'original_size': len(json.dumps(messages))
                }
            )
3. Integration với ADK Patterns và Browser APIs
3.1 Browser-Based Session Service
pythonfrom google.adk.sessions import BaseSessionService

class BrowserSessionService(BaseSessionService):
    """Session service using browser storage"""
    
    def __init__(self, memory_system: BrowserBasedMemory):
        super().__init__()
        self.memory = memory_system
        
    async def save_session(self, session_id: str, session_data: Dict):
        """Save session using browser storage tiers"""
        # Separate hot và cold data
        hot_data, warm_data, cold_data = self._categorize_data(session_data)
        
        # Store in appropriate tiers
        for key, value in hot_data.items():
            self.memory.hot_memory[f"{session_id}:{key}"] = value
            
        for key, value in warm_data.items():
            await self.memory.storage.local_storage_set(
                f"mm_a2a:session:{session_id}:{key}",
                value
            )
            
        for key, value in cold_data.items():
            await self.memory.storage.indexed_db_put(
                'sessions',
                f"{session_id}:{key}",
                value
            )
    
    async def load_session(self, session_id: str) -> Dict:
        """Load session from browser storage tiers"""
        session_data = {}
        
        # Load from hot memory
        for key, value in self.memory.hot_memory.items():
            if key.startswith(f"{session_id}:"):
                field_name = key.split(':', 1)[1]
                session_data[field_name] = value
        
        # Load from localStorage
        js_code = f"""
        const keys = Object.keys(localStorage);
        const sessionData = {{}};
        keys.forEach(key => {{
            if (key.startsWith('mm_a2a:session:{session_id}:')) {{
                const fieldName = key.substring('mm_a2a:session:{session_id}:'.length);
                sessionData[fieldName] = JSON.parse(localStorage.getItem(key));
            }}
        }});
        return sessionData;
        """
        warm_data = await execute_js(js_code)
        session_data.update(warm_data)
        
        # Load from IndexedDB
        db_data = await self._load_from_indexed_db(session_id)
        session_data.update(db_data)
        
        return session_data
    
    def _categorize_data(self, data: Dict) -> Tuple[Dict, Dict, Dict]:
        """Categorize data into hot, warm, and cold"""
        hot_keys = ['current_message', 'active_workflow', 'temp_data']
        warm_keys = ['cart_id', 'user_preferences', 'recent_products']
        
        hot_data = {k: v for k, v in data.items() if k in hot_keys}
        warm_data = {k: v for k, v in data.items() if k in warm_keys}
        cold_data = {k: v for k, v in data.items() 
                     if k not in hot_keys and k not in warm_keys}
        
        return hot_data, warm_data, cold_data
3.2 Memory-Aware Runner với Browser Integration
pythonfrom google.adk.runners import Runner

# Initialize browser-based components
memory_system = BrowserBasedMemory()
session_service = BrowserSessionService(memory_system)
scheduler = BrowserMemoryScheduler(memory_system)

# Start background tasks
asyncio.create_task(scheduler.start())

# Create runner với custom session service
runner = Runner(
    agent=root_agent,
    app_name="mm_a2a_ecommerce",
    session_service=session_service,
    # Limit context window
    include_contents='recent:20',  # Only last 20 messages
)

# Add browser-specific middleware
runner.add_middleware(BrowserStorageMiddleware())
runner.add_middleware(QuotaMonitoringMiddleware())
3.3 Browser Storage Initialization
pythonasync def initialize_browser_storage():
    """Initialize IndexedDB schema"""
    js_code = """
    const request = indexedDB.open('MM_A2A_DB', 1);
    
    request.onupgradeneeded = function(event) {
        const db = event.target.result;
        
        // Create object stores
        if (!db.objectStoreNames.contains('conversations')) {
            db.createObjectStore('conversations', { keyPath: 'key' });
        }
        
        if (!db.objectStoreNames.contains('sessions')) {
            db.createObjectStore('sessions', { keyPath: 'key' });
        }
        
        if (!db.objectStoreNames.contains('summaries')) {
            const summaryStore = db.createObjectStore('summaries', { keyPath: 'key' });
            summaryStore.createIndex('timestamp', 'timestamp', { unique: false });
        }
    };
    
    request.onsuccess = function() {
        console.log('IndexedDB initialized successfully');
    };
    """
    await execute_js(js_code)
4. Implementation Roadmap for Browser-Based Storage
Phase 1: Browser Storage Setup (Week 1)

Implement browser storage interfaces
Setup IndexedDB schema
Create localStorage management

Phase 2: Memory System Migration (Week 2)

Implement BrowserBasedMemory class
Add compression/decompression
Create tiered storage logic

Phase 3: Background Tasks (Week 3)

Implement service worker for background sync
Setup periodic cleanup tasks
Add summarization scheduling

Phase 4: Testing & Optimization (Week 4)

Test storage quotas and limits
Optimize compression algorithms
Add monitoring and analytics

5. Browser Storage Considerations
Storage Limits

localStorage: 5-10MB per origin
IndexedDB: 50% of free disk space (Chrome), varies by browser
In-memory: Limited by browser's JavaScript heap

Browser Compatibility
javascript// Feature detection
const features = {
    localStorage: typeof localStorage !== 'undefined',
    indexedDB: 'indexedDB' in window,
    compressionStream: 'CompressionStream' in window,
    serviceWorker: 'serviceWorker' in navigator,
    backgroundSync: 'SyncManager' in window
};

// Fallback strategies for unsupported features
if (!features.compressionStream) {
    // Use alternative compression library
}
6. Monitoring và Metrics for Browser
pythonclass BrowserStorageMetrics:
    """Track browser storage usage and performance"""
    
    async def get_storage_usage(self) -> Dict:
        """Get current storage usage"""
        js_code = """
        if ('storage' in navigator && 'estimate' in navigator.storage) {
            const estimate = await navigator.storage.estimate();
            return {
                usage: estimate.usage,
                quota: estimate.quota,
                percentUsed: (estimate.usage / estimate.quota) * 100
            };
        }
        return null;
        """
        return await execute_js(js_code)
    
    async def track_operation(self, operation: str, tier: str, duration: float):
        """Track storage operations"""
        # Store metrics in IndexedDB for later analysis
        metric = {
            'operation': operation,
            'tier': tier,
            'duration': duration,
            'timestamp': datetime.utcnow().isoformat()
        }
        
        await BrowserStorageInterface.indexed_db_put(
            'metrics',
            f"metric_{datetime.utcnow().timestamp()}",
            metric
        )
7. Security Considerations
pythonclass StorageSecurity:
    """Security measures for browser storage"""
    
    @staticmethod
    async def encrypt_sensitive_data(data: Any, key: str) -> str:
        """Encrypt sensitive data before storage"""
        js_code = f"""
        const encoder = new TextEncoder();
        const data = encoder.encode(JSON.stringify({json.dumps(data)}));
        
        const key = await crypto.subtle.importKey(
            'raw',
            encoder.encode('{key}'),
            'AES-GCM',
            false,
            ['encrypt']
        );
        
        const iv = crypto.getRandomValues(new Uint8Array(12));
        const encrypted = await crypto.subtle.encrypt(
            {{ name: 'AES-GCM', iv: iv }},
            key,
            data
        );
        
        // Combine IV and encrypted data
        const combined = new Uint8Array(iv.length + encrypted.byteLength);
        combined.set(iv, 0);
        combined.set(new Uint8Array(encrypted), iv.length);
        
        return btoa(String.fromCharCode.apply(null, combined));
        """
        return await execute_js(js_code)
Conclusion
Giải pháp này adapt kiến trúc tiered storage cho browser environment, sử dụng:

In-memory cho hot data
localStorage cho warm data
IndexedDB cho cold data

Với các browser APIs hiện đại, hệ thống có thể hoạt động hiệu quả trong browser environment trong khi vẫn maintain được performance và scalability.