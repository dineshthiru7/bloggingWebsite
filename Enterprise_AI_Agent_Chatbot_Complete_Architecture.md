# Enterprise AI Agent Chatbot — Complete Architecture

## 1. Scope

Generic enterprise AI chatbot/agent platform. No APEX, Java, or Spring Boot dependency.

Core stack:

- React + TypeScript
- AG-UI
- API Gateway
- Agent Runtime
- LangGraph
- Jev
- Gemini / Claude / other LLMs
- RAG / Hybrid Search / Graph RAG
- MCP Gateway + MCP Servers
- PostgreSQL + Redis + Vector DB + Elasticsearch + Graph DB + Object Storage
- OpenTelemetry + LangSmith + evaluation
- Human-in-the-loop approvals

---

# 2. Complete Architecture

```text
                              USER
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         EXPERIENCE / UI                                     │
│ React + TypeScript + AG-UI + Tailwind + shadcn/ui                          │
│ Chat │ Conversations │ Files │ Citations │ Activity │ Approvals │ Artifacts│
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                         HTTPS / SSE / WebSocket
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           API GATEWAY                                       │
│ Auth │ RBAC/ABAC │ Tenant │ Rate Limit │ Validation │ Routing │ CORS        │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         AGENT RUNTIME                                       │
│ Agent Registry │ Session Manager │ Execution Manager │ Context Manager     │
│ Policy Engine  │ Approval Manager │ Event Publisher                        │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          LANGGRAPH                                          │
│ State │ Checkpoints │ Routing │ Retry │ Interrupt │ Resume │ Subgraphs      │
│                                                                             │
│                         Supervisor                                          │
│                    ┌────────┼────────┐                                     │
│                    ▼        ▼        ▼                                     │
│                 Research  Coding  Analysis                                 │
│                   Agent    Agent    Agent                                   │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                       ┌──────────┴──────────┐
                       ▼                     ▼
                ┌─────────────┐       ┌─────────────┐
                │     JEV     │       │     LLM     │
                │ Decisions   │       │ Reasoning   │
                │ Routing     │       │ Generation  │
                │ Tool Choice │       │ Code        │
                │ Completion  │       │ Analysis    │
                │ Retry/Risk  │       │ Summary     │
                └──────┬──────┘       └──────┬──────┘
                       │                     │
                       └──────────┬──────────┘
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CONTEXT / RAG                                       │
│ Vector │ Hybrid │ Graph RAG │ Reranker │ Context Fusion                    │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          MCP GATEWAY                                        │
│ Tool Registry │ Authorization │ Tenant Policy │ Validation │ Audit          │
└─────────────────────────────────┬───────────────────────────────────────────┘
                                  │
                   ┌──────────────┼──────────────┐
                   ▼              ▼              ▼
                GitHub           Jira        Enterprise APIs
                   │              │              │
                   └──────────────┼──────────────┘
                                  ▼
                         Enterprise Systems

┌─────────────────────────────────────────────────────────────────────────────┐
│ DATA: PostgreSQL │ Redis │ pgvector │ Elasticsearch │ Graph DB │ Storage   │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│ OBSERVABILITY: OpenTelemetry │ LangSmith │ Evaluation │ Metrics │ Audit     │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 3. Responsibility Model

| Component | Responsibility |
|---|---|
| React | Visual UI |
| AG-UI | Agent ↔ UI communication/events |
| API Gateway | Security, tenant, routing, rate limits |
| Agent Runtime | Agent lifecycle and execution management |
| LangGraph | Workflow, state, checkpoint, retry, interrupt/resume |
| Jev | Bounded decisions |
| LLM | Open-ended reasoning and generation |
| RAG | Knowledge retrieval |
| MCP Gateway | Tool governance |
| Policy Engine | Authorization |
| PostgreSQL | Durable state |
| Redis | Transient state/cache |
| Vector DB | Semantic retrieval |
| Elasticsearch | Keyword/hybrid retrieval |
| Graph DB | Relationship retrieval |
| Object Storage | Files/artifacts |
| OpenTelemetry | Observability |

---

# 4. Frontend Architecture

## Recommended stack

```text
React
TypeScript
AG-UI Client
Tailwind CSS
shadcn/ui
Radix UI
React Query
Zustand (or equivalent)
```

## Component tree

```text
src/
├── app/
├── components/
│   ├── chat/
│   │   ├── ChatShell
│   │   ├── ChatHeader
│   │   ├── ChatInput
│   │   ├── MessageList
│   │   ├── UserMessage
│   │   ├── AssistantMessage
│   │   └── StreamingMessage
│   ├── agent/
│   │   ├── AgentSelector
│   │   ├── AgentActivity
│   │   ├── ExecutionTimeline
│   │   ├── ToolCallCard
│   │   └── SubAgentCard
│   ├── files/
│   ├── citations/
│   ├── approvals/
│   └── artifacts/
├── hooks/
│   ├── useAGUI
│   ├── useAgentRun
│   ├── useStreaming
│   └── useApproval
├── services/
│   ├── apiClient
│   └── aguiClient
└── state/
    ├── chatStore
    ├── sessionStore
    └── executionStore
```

## Enterprise UI

```text
┌──────────────────────────────────────────────────────────────────────┐
│ AI Assistant                              Agent: Research ▼  Settings │
├────────────────┬──────────────────────────────┬──────────────────────┤
│ Conversations  │            CHAT              │ Agent Activity       │
│                │                              │                      │
│ + New Chat     │ User                         │ ✓ Request parsed     │
│ Today          │ Analyze this document        │ ✓ RAG search         │
│ ├─ Research    │                              │ ✓ 12 sources         │
│ ├─ Project     │ Assistant                    │ ⟳ Graph retrieval    │
│ └─ Incident    │ ┌────────────────────────┐  │ ○ Generate           │
│                │ │ Streaming answer...     │  │                      │
│ Yesterday      │ └────────────────────────┘  │ Tool Calls           │
│ ├─ Coding      │                              │ ├─ Search             │
│ └─ Design      │ [Sources] [Tools] [Files]  │ └─ Graph Query        │
│                │                              │                      │
│                │ ┌────────────────────────┐  │                      │
│                │ │ Ask anything...   📎 ➤│  │                      │
│                │ └────────────────────────┘  │                      │
└────────────────┴──────────────────────────────┴──────────────────────┘
```

AG-UI does not dictate the visual design. The UI can be fully customized.

---

# 5. AG-UI + Transport

```text
React
  │
  ▼
AG-UI Client
  │
  ├── HTTP ───────────────► API Gateway
  │
  ├── SSE ────────────────► API Gateway
  │
  └── WebSocket ──────────► API Gateway
                                  │
                                  ▼
                            Agent Runtime
```

Use HTTP for normal APIs.

Use SSE when the server primarily streams events to the browser.

Use WebSocket for genuine bidirectional live sessions.

Do not make WebSocket a mandatory middle tier.

---

# 6. AG-UI Event Flow

```text
RUN_STARTED
    ↓
STEP_STARTED
    ↓
TOOL_CALL_START
    ↓
TOOL_CALL_END
    ↓
STATE_UPDATE
    ↓
TEXT_MESSAGE_START
    ↓
TEXT_MESSAGE_CONTENT
    ↓
TEXT_MESSAGE_END
    ↓
STEP_FINISHED
    ↓
RUN_FINISHED
```

UI mapping:

```text
TOOL_CALL_START → Tool card "Running..."
TOOL_CALL_END   → Tool card "Completed"
STATE_UPDATE    → Activity/timeline update
TEXT_CONTENT    → Streaming assistant message
RUN_FINISHED    → Final response state
```

Do not expose private model chain-of-thought.

---

# 7. API Gateway

```text
Internet
   ↓
Load Balancer
   ↓
API Gateway
   ├── Authentication
   ├── Authorization
   ├── RBAC / ABAC
   ├── Tenant
   ├── Rate Limit
   ├── Request Validation
   ├── API Versioning
   ├── CORS
   └── Routing
   ↓
Agent Runtime
```

The gateway is not the reasoning layer.

---

# 8. Agent Control Plane

```text
                    Agent Control Plane
                           │
          ┌────────────────┼─────────────────┐
          ▼                ▼                 ▼
    Agent Registry    Session Manager   Execution Manager
          │                │                 │
          ▼                ▼                 ▼
    Agent Config      Conversation       Run State
    Agent Version     Context            Retry
    Model Config      User Context       Resume
    Tool Policy
```

Additional:

```text
Context Manager
Policy Engine
Approval Manager
Event Publisher
```

---

# 9. Agent Registry

Agent definitions should be configurable and versioned.

```text
AgentDefinition
├── Identity
├── Version
├── Instructions
├── Model
├── Tools
├── MCP Permissions
├── RAG Configuration
├── Jev Configuration
├── Memory Configuration
├── Retry Policy
├── Approval Policy
├── Limits
└── Observability
```

Example:

```json
{
  "agentId": "research-agent",
  "version": "2.1",
  "model": {
    "provider": "anthropic",
    "name": "claude"
  },
  "tools": [
    "knowledge.search",
    "web.search"
  ],
  "mcpServers": [
    "enterprise-search"
  ],
  "rag": {
    "enabled": true,
    "mode": "hybrid"
  },
  "jev": {
    "routing": true,
    "toolSelection": true,
    "completion": true,
    "retryDecision": true
  },
  "limits": {
    "maxIterations": 12,
    "timeoutSeconds": 600
  }
}
```

---

# 10. Agent Runtime

```text
Agent Runtime
├── Agent Registry
├── Agent Router
├── Session Manager
├── Execution Manager
├── Context Manager
├── Policy Engine
├── Approval Manager
├── Event Publisher
└── LangGraph Runtime
```

Execution states:

```text
CREATED
 ↓
QUEUED
 ↓
RUNNING
 ↓
WAITING_TOOL
 ↓
RUNNING
 ↓
WAITING_APPROVAL
 ↓
RUNNING
 ↓
COMPLETED

Terminal:
FAILED
CANCELLED
TIMEOUT
```

---

# 11. LangGraph Architecture

```text
START
  ↓
Load Session
  ↓
Build Context
  ↓
Route
  ↓
Jev
  ↓
┌──────────────┬──────────────┬──────────────┐
▼              ▼              ▼
Research       Coding         Analysis
Agent          Agent          Agent
└──────────────┴──────────────┴──────────────┘
                 ↓
                LLM
                 ↓
              Tool Call
                 ↓
             Observation
                 ↓
                Jev
                 ↓
        ┌────────┼─────────┐
        ▼        ▼         ▼
      Retry   Continue   Complete
        │        │         │
        └────────┘         ▼
                           END
```

LangGraph owns:

- State
- Checkpoints
- Workflow routing
- Conditional edges
- Retry
- Interrupt/resume
- Subgraphs
- Long-running execution
- Human-in-the-loop orchestration

---

# 12. LangGraph State

```json
{
  "runId": "run-001",
  "sessionId": "session-001",
  "agentId": "research-agent",
  "status": "RUNNING",
  "messages": [],
  "plan": [],
  "context": [],
  "toolCalls": [],
  "observations": [],
  "approval": null,
  "iteration": 3
}
```

Persist checkpoints for long-running or approval workflows.

---

# 13. Jev Architecture

Jev is a decision layer inside LangGraph.

```text
LangGraph State
      ↓
     Jev
      │
 ┌────┼──────────────┐
 ▼    ▼              ▼
Route Tool Choice  Completion
 │    │              │
 ▼    ▼              ▼
Agent Tool       Stop/Continue
```

Good decision points:

```text
Jev
├── Agent routing
├── Tool selection
├── Retrieval strategy
├── Completion
├── Retry
├── Strategy change
├── Risk classification
└── Escalation
```

Do not make Jev the overall workflow engine.

---

# 14. LLM Architecture

```text
                 Model Router
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       Gemini       Claude      Other
```

LLM responsibilities:

- Reasoning
- Generation
- Planning
- Code generation
- Summarization
- Requirement analysis
- Explanation

Simple distinction:

```text
Jev → "What decision should be made?"

LLM → "What should be reasoned/generated?"
```

---

# 15. Agent Loop

```text
               State
                 ↓
                Jev
                 ↓
          Select next action
                 ↓
                LLM
                 ↓
               Tool
                 ↓
             Observation
                 ↓
                Jev
                 ↓
        ┌────────┼────────┐
        ▼        ▼        ▼
      Retry   Continue  Complete
        │        │        │
        └────────┘        ▼
                          END
```

---

# 16. Multi-Agent Architecture

```text
                         Supervisor
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
      Research             Coding             Analysis
       Agent                Agent               Agent
          │                   │                   │
          ▼                   ▼                   ▼
       RAG/Web             Code Tools          Data Tools
          │                   │                   │
          └───────────────────┼───────────────────┘
                              ▼
                         Review Agent
                              │
                              ▼
                         Final Result
```

Each specialist can be a LangGraph subgraph.

---

# 17. Context / RAG Architecture

```text
                         Query
                           ↓
                          Jev
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
           Vector        Hybrid        Graph
           Search        Search        RAG
              │            │            │
              ▼            ▼            ▼
          pgvector    Elasticsearch   Graph DB
              │            │            │
              └────────────┼────────────┘
                           ↓
                        Reranker
                           ↓
                      Context Fusion
                           ↓
                          LLM
```

---

# 18. RAG Ingestion

```text
Documents
   ↓
Parser
   ↓
Chunker
   ↓
Metadata
   ├─────────────┐
   ▼             ▼
Embeddings    Graph Extraction
   │             │
   ▼             ▼
Vector DB      Graph DB

Keyword Index
   ↓
Elasticsearch
```

---

# 19. Hybrid Search

```text
Query
 ├───────────────┐
 ▼               ▼
Vector Search   Keyword Search
 │               │
 ▼               ▼
Semantic        Exact
 └───────┬───────┘
         ▼
       Fusion
         ↓
      Reranker
         ↓
    Final Context
```

---

# 20. Graph RAG

```text
Query
 ↓
Entity Extraction
 ↓
Graph Query
 ↓
┌────────────┬────────────┬────────────┐
▼            ▼            ▼
Service     Document     Dependency
  │            │            │
  └────────────┼────────────┘
               ▼
          Graph Context
```

---

# 21. MCP Gateway

```text
LangGraph
    ↓
MCP Gateway
    ├── Tool Registry
    ├── Authentication
    ├── Authorization
    ├── Tenant Policy
    ├── Tool Filtering
    ├── Rate Limiting
    ├── Input Validation
    ├── Output Validation
    └── Audit
    ↓
MCP Servers
```

MCP tools:

```text
GitHub
├── search
├── read
├── create-branch
└── create-pr

Jira
├── search
├── get-issue
└── update-issue

Database
├── schema
└── controlled-query

Kubernetes
├── pods
├── logs
└── controlled-operation

Internal APIs
└── enterprise capabilities
```

---

# 22. Tool Authorization

```text
User
 ↓
Identity
 ↓
RBAC / ABAC
 ↓
Agent Policy
 ↓
Tool Policy
 ↓
MCP Gateway
 ↓
Enterprise Tool
```

Example:

```text
GitHub Search       ✓
GitHub Read         ✓
Create Branch       ✓
Create PR           ✓
Merge PR            ✗
Production DB       ✗
Production Deploy   ✗
```

The LLM must never be the authorization authority.

---

# 23. Human Approval

```text
Agent
 ↓
Action
 ↓
Policy Engine
 ↓
Approval Required?
 ├── NO  → Execute
 │
 └── YES
      ↓
 WAITING_APPROVAL
      ↓
 AG-UI
      ↓
 Approval Card
      ↓
 ┌────┴────┐
 ▼         ▼
Approve   Reject
 ↓         ↓
Resume    Stop
```

Example UI:

```text
┌──────────────────────────────────────────────┐
│ ⚠ Human Approval Required                   │
├──────────────────────────────────────────────┤
│ Action: Create Pull Request                 │
│ Repository: payments-service                │
│ Branch: feature/1234                        │
│ Files: 8                                    │
│ Tests: 42 passed                            │
│ Risk: MEDIUM                                │
│                                              │
│ [ View Diff ]                                │
│                                              │
│       [ Reject ]      [ Approve ]            │
└──────────────────────────────────────────────┘
```

---

# 24. Memory Architecture

Keep workflow state, memory and knowledge separate.

```text
                    State / Memory
                          │
          ┌───────────────┼────────────────┐
          ▼               ▼                ▼
    Workflow State   Session Memory   Long-Term Memory
          │               │                │
      LangGraph          Redis         PostgreSQL
```

Knowledge:

```text
Vector DB
Elasticsearch
Graph DB
Object Storage
```

---

# 25. Conversation Architecture

```text
Conversation
├── User
├── Session
├── Messages
├── Attachments
├── Agent Runs
│   ├── Steps
│   ├── Tool Calls
│   ├── Approvals
│   └── Artifacts
└── Metadata
```

Relational model:

```text
conversation
 ├── message
 ├── attachment
 └── execution
      ├── execution_step
      ├── tool_call
      ├── approval
      └── artifact
```

---

# 26. File Upload Pipeline

```text
React
 ↓
API Gateway
 ↓
Upload Service
 ↓
Object Storage
 ↓
Document Processor
 ├── Parse
 ├── OCR
 ├── Chunk
 ├── Metadata
 ├── Embedding
 └── Graph Extraction
 ↓
┌─────────────┬────────────────┬─────────────┐
▼             ▼                ▼
pgvector  Elasticsearch      Graph DB
```

---

# 27. Agent Activity UI

Do not expose private chain-of-thought.

Expose operational status:

```text
┌──────────────────────────────────┐
│ Agent Activity                   │
├──────────────────────────────────┤
│ ✓ Understand request             │
│ ✓ Search knowledge               │
│ ✓ Retrieve 12 sources            │
│ ✓ Analyze context                │
│ ⟳ Generate response              │
│ ○ Validate response              │
└──────────────────────────────────┘
```

---

# 28. Tool Activity UI

```text
┌──────────────────────────────────────┐
│ 🔧 GitHub Search                     │
├──────────────────────────────────────┤
│ Repository: payments-service         │
│ Query: PaymentService                │
│                                      │
│ ✓ Completed                          │
│ 18 files found                       │
│                                      │
│ [ View Results ]                     │
└──────────────────────────────────────┘
```

---

# 29. Citation Architecture

```text
LLM
 ↓
Context Sources
 ↓
Citation Resolver
 ↓
AG-UI
 ↓
Citation Component
```

Example:

```text
Answer:

The service uses event-driven communication.

Sources:
[1] Architecture.pdf — Page 18
[2] Service Design.md — Section 4.2
[3] PaymentService.java
```

---

# 30. Artifact Architecture

```text
Agent
 ↓
Artifact Service
 ↓
Object Storage
 ↓
Artifact Metadata
 ↓
AG-UI
 ↓
React Viewer
```

Supported artifacts:

```text
Code
Documents
Reports
JSON
CSV
Charts
Diagrams
SQL
```

---

# 31. Error and Retry

```text
Tool Error
    ↓
LangGraph
    ↓
Jev
    │
 ┌──┼─────────────┐
 ▼  ▼             ▼
Retry Change Tool Escalate
 │    │             │
 ▼    ▼             ▼
Tool Tool          Human
```

Suggested limits:

```text
maxToolRetries = 2
maxAgentIterations = 10
maxExecutionTime = 10 minutes
```

---

# 32. Cancellation

```text
React
 ↓
AG-UI / HTTP
 ↓
API Gateway
 ↓
Execution Manager
 ↓
LangGraph
 ↓
Interrupt / Cancel
```

UI:

```text
Generating...
[ Stop ]
```

---

# 33. Security Architecture

```text
User
 ↓
Identity Provider
 ↓
API Gateway
 ↓
RBAC / ABAC
 ↓
Agent Policy
 ↓
Tool Policy
 ↓
MCP Gateway
 ↓
Enterprise System
```

Requirements:

```text
OAuth2 / OIDC
RBAC
ABAC where required
Tenant isolation
Secrets manager
Credential isolation
Encryption in transit
Encryption at rest
Audit
Input validation
Output validation
Prompt injection defense
Tool permission boundaries
```

---

# 34. Prompt Injection Boundary

Treat external content as untrusted.

```text
Web / Document / Tool Result
          ↓
      Sanitization
          ↓
     Context Builder
          ↓
        Policy
          ↓
          LLM
```

Tool results must not override system-level policy.

---

# 35. Tenant Isolation

Trusted execution context:

```text
tenantId
userId
sessionId
runId
agentId
```

Tenant boundaries apply to:

```text
Conversations
Files
Memory
Knowledge
Agents
Tools
MCP permissions
Artifacts
Audit logs
```

---

# 36. Observability

```text
                    Agent Runtime
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
          Metrics       Logs        Traces
             │           │           │
             └───────────┼───────────┘
                         ▼
                  OpenTelemetry
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
          LangSmith   Metrics     Logs
```

Track:

```text
runId
tenantId
userId
agentId
agentVersion
model
promptVersion
latency
tokens
toolCalls
retrievalLatency
retrievalSources
JevDecision
retryCount
approvalTime
status
cost
```

---

# 37. Evaluation

```text
Agent
 ↓
Evaluation
 ├── Correctness
 ├── Relevance
 ├── Groundedness
 ├── Tool Selection
 ├── Retrieval Quality
 ├── Completion Accuracy
 ├── Safety
 └── Cost / Latency
```

Dataset:

```text
Normal
Ambiguous
Failure
Tool Failure
Security
Hallucination
Adversarial
```

---

# 38. Agent Lifecycle

```text
Draft
 ↓
Development
 ↓
Evaluation
 ↓
Security Review
 ↓
Approved
 ↓
Published
 ↓
Production
 ↓
Deprecated
```

Version:

```text
research-agent:v1
research-agent:v2
research-agent:v3
```

Support rollback.

---

# 39. Deployment Architecture

```text
Internet
   ↓
Load Balancer
   ↓
API Gateway
   ↓
┌────────────┬────────────┬────────────┐
▼            ▼            ▼
Runtime-1   Runtime-2   Runtime-N
└────────────┬────────────┘
             ▼
          LangGraph
             │
     ┌───────┼────────┐
     ▼       ▼        ▼
    LLM     RAG      MCP
```

Kubernetes:

```text
Kubernetes
├── ingress
├── api-gateway
├── agent-runtime
├── context-service
├── mcp-gateway
├── document-service
├── approval-service
└── observability
```

External:

```text
PostgreSQL
Redis
Vector DB
Elasticsearch
Graph DB
Object Storage
Identity Provider
LLM Providers
```

---

# 40. Scaling

For long-running executions:

```text
API Gateway
    ↓
Execution Queue
    ↓
┌──────────┬──────────┬──────────┐
▼          ▼          ▼
Worker 1   Worker 2   Worker N
└──────────┴──────────┴──────────┘
             ↓
          LangGraph
```

Use synchronous execution for simple Q&A.

Use asynchronous execution for:

- Long research
- Large documents
- Coding workflows
- Multi-agent tasks
- Human approval workflows

---

# 41. End-to-End Chat Request

Example:

> Analyze this architecture document and identify risks.

```text
1. User enters request
2. React creates AG-UI run
3. API Gateway authenticates
4. Agent Runtime loads agent
5. LangGraph initializes state
6. Jev selects retrieval strategy
7. Context Service retrieves knowledge
8. Reranker selects context
9. LLM analyzes
10. Jev checks completion
11. LangGraph validates
12. Agent Runtime emits AG-UI events
13. React streams answer
14. Citations render
15. Execution is persisted
```

---

# 42. End-to-End Tool Request

Example:

> Create a Jira ticket.

```text
User
 ↓
React
 ↓
AG-UI
 ↓
API Gateway
 ↓
Agent Runtime
 ↓
LangGraph
 ↓
LLM prepares ticket
 ↓
Jev selects Jira tool
 ↓
Policy checks permission
 ↓
Approval?
 ↓
AG-UI Approval Card
 ↓
User Approves
 ↓
LangGraph Resume
 ↓
MCP Gateway
 ↓
Jira MCP
 ↓
Jira
 ↓
Tool Result
 ↓
LangGraph
 ↓
AG-UI
 ↓
React
```

---

# 43. End-to-End Research Request

```text
User
 ↓
Research Agent
 ↓
LangGraph
 ↓
Jev
 ↓
Retrieval Strategy
 ↓
Vector / Hybrid / Graph
 ↓
Reranker
 ↓
LLM
 ↓
Citation Validation
 ↓
Jev Completion
 ↓
Final Answer
 ↓
AG-UI
 ↓
React
```

---

# 44. End-to-End Human Approval

```text
Agent
 ↓
Action
 ↓
Policy Engine
 ↓
HIGH RISK
 ↓
WAITING_APPROVAL
 ↓
AG-UI
 ↓
React Approval Card
 ↓
User Approves
 ↓
Approval API
 ↓
Execution Manager
 ↓
LangGraph Resume
 ↓
MCP Gateway
 ↓
Tool
 ↓
Result
 ↓
Final Response
```

---

# 45. Repository Structure

```text
enterprise-agent-platform/
│
├── frontend/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   ├── state/
│   └── routes/
│
├── gateway/
│   ├── auth/
│   ├── routing/
│   ├── middleware/
│   └── rate-limit/
│
├── agent-runtime/
│   ├── registry/
│   ├── sessions/
│   ├── execution/
│   ├── policy/
│   ├── approval/
│   └── events/
│
├── agents/
│   ├── supervisor/
│   ├── research/
│   ├── coding/
│   └── analysis/
│
├── orchestration/
│   ├── langgraph/
│   ├── state/
│   ├── checkpoints/
│   └── workflows/
│
├── decision/
│   └── jev/
│
├── context/
│   ├── retrieval/
│   ├── reranking/
│   ├── hybrid/
│   └── graph-rag/
│
├── mcp/
│   ├── gateway/
│   ├── registry/
│   └── servers/
│
├── documents/
│   ├── parser/
│   ├── chunking/
│   ├── embedding/
│   └── indexing/
│
├── observability/
│   ├── tracing/
│   ├── metrics/
│   ├── logging/
│   └── evaluation/
│
└── infrastructure/
    ├── docker/
    ├── kubernetes/
    └── terraform/
```

---

# 46. API Surface

## Conversations

```http
POST   /api/v1/conversations
GET    /api/v1/conversations
GET    /api/v1/conversations/{id}
DELETE /api/v1/conversations/{id}
```

## Runs

```http
POST /api/v1/conversations/{id}/runs
GET  /api/v1/runs/{id}
POST /api/v1/runs/{id}/cancel
GET  /api/v1/runs/{id}/events
```

## Agents

```http
GET /api/v1/agents
GET /api/v1/agents/{id}
```

## Approvals

```http
GET  /api/v1/approvals
POST /api/v1/approvals/{id}/approve
POST /api/v1/approvals/{id}/reject
```

## Files

```http
POST   /api/v1/files
GET    /api/v1/files/{id}
DELETE /api/v1/files/{id}
```

---

# 47. Example Run Request

```json
{
  "agentId": "research-agent",
  "message": "Analyze this architecture",
  "attachments": [
    {
      "fileId": "file-123"
    }
  ],
  "options": {
    "stream": true
  }
}
```

---

# 48. Example Agent Event

```json
{
  "runId": "run-123",
  "event": "TOOL_CALL_END",
  "timestamp": "2026-09-28T10:00:00Z",
  "tool": {
    "name": "knowledge.search",
    "status": "completed"
  },
  "metadata": {
    "resultCount": 12
  }
}
```

---

# 49. Recommended Service Boundaries

```text
1. API Gateway
2. Agent Runtime
3. Agent Registry
4. Context/RAG Service
5. MCP Gateway
6. Document Processing Service
7. Approval/Policy Service
8. Notification/Event Service
9. Observability
```

Do not split everything into microservices on day one. Start with logical boundaries and physically separate services where scale, security, ownership or deployment requirements justify it.

---

# 50. Recommended Technology Stack

## Frontend

```text
React
TypeScript
AG-UI
Tailwind
shadcn/ui
Radix
```

## Agent Runtime

```text
Python
FastAPI
LangGraph
```

## Decision

```text
Jev
```

## Models

```text
Gemini
Claude
Other enterprise LLMs
```

## Knowledge

```text
pgvector
Elasticsearch
Graph DB
Reranker
```

## Tools

```text
MCP
MCP Gateway
MCP Servers
```

## State

```text
PostgreSQL
Redis
LangGraph checkpoints
```

## Infrastructure

```text
Docker
Kubernetes
Object Storage
API Gateway / Ingress
Secrets Manager
Identity Provider
```

## Observability

```text
OpenTelemetry
LangSmith
Evaluation
Metrics
Logs
Traces
Audit
```

---

# 51. Final Mental Model

```text
                         USER
                           │
                           ▼
                      React UI
                           │
                         AG-UI
                           │
                           ▼
                     API Gateway
                           │
                           ▼
                    Agent Runtime
                           │
                           ▼
                       LangGraph
                      /         \
                     /           \
                   Jev           LLM
                    │             │
               Decisions     Reasoning
                    \             /
                     \           /
                       Context
                          │
                         RAG
                          │
                     MCP Gateway
                          │
                        Tools
                          │
                          ▼
                 Enterprise Systems
```

---

# 52. Final Architecture Rule

The clean separation is:

```text
AG-UI
  ↓
UI ↔ Agent communication

API Gateway
  ↓
Secure entry point

Agent Runtime
  ↓
Agent lifecycle

LangGraph
  ↓
Workflow + state

Jev
  ↓
Bounded decisions

LLM
  ↓
Reasoning + generation

RAG
  ↓
Knowledge

MCP Gateway
  ↓
Actions/tools

Policy Engine
  ↓
Authorization

PostgreSQL / Redis
  ↓
State + memory

OpenTelemetry / LangSmith
  ↓
Observability
```

This makes the platform modular: the UI, LLM provider, decision model, vector store, graph store, tools, and individual agents can evolve independently.
