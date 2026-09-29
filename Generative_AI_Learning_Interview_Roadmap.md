# Generative AI --- Learning & Interview Roadmap

## 🎯 Goal

This roadmap is designed for **Generative AI Engineer / Forward Deployed
AI Engineer / AI Architect / Senior GenAI Engineer** interviews.

The focus is on:

-   LLM fundamentals
-   Transformers
-   Embeddings
-   RAG
-   Vector databases
-   Prompt engineering
-   Tool calling
-   AI Agents
-   LangGraph
-   MCP
-   Multi-agent systems
-   Evaluation
-   Observability
-   Security
-   LLM inference
-   AI system design
-   Production GenAI architecture

------------------------------------------------------------------------

# 1. LLM Fundamentals ⭐⭐⭐⭐⭐

You should be able to explain these clearly from first principles.

## Topics

-   What is an LLM?
-   Tokens and tokenization
-   Embeddings
-   Context window
-   Parameters
-   Pre-training vs fine-tuning
-   Instruction tuning
-   RLHF / RLAIF
-   Transformer architecture
-   Encoder vs Decoder vs Encoder-Decoder
-   Self-attention
-   Multi-head attention
-   Causal / masked attention
-   Cross-attention
-   Positional encoding
-   Feed-forward network
-   Layer normalization
-   Residual connections
-   Temperature
-   Top-K
-   Top-P
-   Greedy decoding vs sampling
-   Hallucination

## Core Flow

``` text
Input
  ↓
Tokenization
  ↓
Token Embeddings
  ↓
Transformer
  ↓
Attention
  ↓
Next-token prediction
  ↓
Sampling / Decoding
  ↓
Output
```

### Interview must-know

> Explain how an LLM generates the next token.

------------------------------------------------------------------------

# 2. Transformers ⭐⭐⭐⭐⭐

## Architecture

``` text
Input
  ↓
Tokenization
  ↓
Token Embeddings
  ↓
Positional Information
  ↓
Transformer Layers
  │
  ├── Multi-Head Self Attention
  ├── Add & Norm
  ├── Feed Forward Network
  └── Add & Norm
  ↓
Output Logits
  ↓
Softmax
  ↓
Next Token
```

## Learn deeply

-   Query, Key, Value
-   Attention formula
-   Scaled dot-product attention
-   Why divide by √dₖ?
-   Multi-head attention
-   Causal attention
-   KV cache
-   Encoder-only models
-   Decoder-only models
-   Encoder-decoder models
-   Transformer inference
-   Residual connections
-   Layer normalization

## Important interview questions

-   What is self-attention?
-   Why do we need Query, Key and Value?
-   What is multi-head attention?
-   Why is attention scaled?
-   What is masked self-attention?
-   Encoder vs decoder?
-   Why does GPT use decoder architecture?
-   What is KV cache?

------------------------------------------------------------------------

# 3. Embeddings ⭐⭐⭐⭐⭐

Embeddings convert text into numerical vectors representing semantic
meaning.

``` text
"This is Thirumurthi"
          ↓
    Embedding Model
          ↓
[0.21, -0.43, 0.82, ...]
```

## Learn

-   What is an embedding?
-   Semantic similarity
-   Cosine similarity
-   Euclidean distance
-   Dot product
-   Dense embeddings
-   Sparse embeddings
-   Embedding dimensions
-   Sentence embeddings
-   Query embeddings
-   Document embeddings
-   Embedding model selection

Example:

``` text
"car"
"automobile"
```

can produce vectors that are close in embedding space because their
meanings are related.

------------------------------------------------------------------------

# 4. RAG --- Retrieval Augmented Generation ⭐⭐⭐⭐⭐⭐⭐

RAG is one of the most important topics for GenAI interviews.

## Basic RAG Architecture

### Offline / Ingestion

``` text
Documents
    ↓
Document Loader
    ↓
Parser
    ↓
Chunking
    ↓
Embedding Model
    ↓
Vector Database
```

### Online / Query

``` text
User Query
    ↓
Query Embedding
    ↓
Retriever
    ↓
Vector Search
    ↓
Top-K Documents
    ↓
Reranker
    ↓
Relevant Context
    ↓
LLM
    ↓
Answer
```

## Learn deeply

### Document ingestion

-   Document loaders
-   PDF parsing
-   HTML parsing
-   Word / Excel processing
-   OCR
-   Metadata extraction

### Chunking

-   Fixed-size chunking
-   Recursive chunking
-   Semantic chunking
-   Parent-child chunking
-   Chunk overlap
-   Chunk size
-   Metadata-based chunking

### Retrieval

-   Dense retrieval
-   Sparse retrieval
-   BM25
-   Vector similarity
-   Top-K
-   Metadata filtering
-   Hybrid search
-   Query rewriting
-   Multi-query retrieval
-   HyDE
-   Context compression

### Reranking

Understand why the pipeline can be:

``` text
Query
 ↓
Vector Search
 ↓
Top 50
 ↓
Reranker
 ↓
Top 5
 ↓
LLM
```

## Important interview questions

-   What is RAG?
-   Why do we need RAG?
-   RAG vs fine-tuning?
-   How do you select chunk size?
-   What is chunk overlap?
-   Why does RAG hallucinate?
-   How do you improve RAG accuracy?
-   What is hybrid search?
-   Why do we need reranking?
-   What is query rewriting?
-   What is HyDE?
-   How do you evaluate RAG?

------------------------------------------------------------------------

# 5. Vector Databases ⭐⭐⭐⭐⭐

Know the concepts behind:

-   Pinecone
-   Qdrant
-   Milvus
-   Chroma
-   FAISS
-   Weaviate

## Core architecture

``` text
Document
   ↓
Embedding
   ↓
Vector
   ↓
Vector Index
   ↓
ANN Search
   ↓
Nearest Neighbors
```

## Important concepts

-   KNN
-   ANN
-   HNSW
-   IVF
-   Product Quantization
-   Vector indexes
-   Similarity metrics
-   Metadata filtering
-   Hybrid search

## KNN

``` text
Query Vector
      ↓
Find K nearest vectors
      ↓
Return top K documents
```

Understand the difference between:

-   Exact KNN
-   Approximate Nearest Neighbor (ANN)

------------------------------------------------------------------------

# 6. Prompt Engineering ⭐⭐⭐⭐

## Learn

-   Zero-shot prompting
-   Few-shot prompting
-   System prompts
-   User prompts
-   Role prompting
-   Structured prompting
-   Prompt templates
-   Output constraints
-   JSON output
-   Few-shot examples
-   Prompt injection

## Production prompting

``` text
Poor Prompt
    ↓
Inconsistent Output

Structured Prompt
    ↓
Controlled Output
```

------------------------------------------------------------------------

# 7. Function Calling / Tool Calling ⭐⭐⭐⭐⭐

Tool calling is the bridge between LLMs and external systems.

``` text
User
 ↓
LLM
 ↓
Decides tool is required
 ↓
Tool Call
 ↓
API / Database / Search
 ↓
Tool Result
 ↓
LLM
 ↓
Final Answer
```

## Learn

-   Function calling
-   Tool schemas
-   Structured outputs
-   Tool selection
-   Tool execution
-   Parallel tool calls
-   Tool result handling
-   Tool errors
-   Retries
-   Tool permissions

Example:

``` text
User:
"What is my order status?"

LLM
 ↓
get_order_status(order_id)
 ↓
Backend API
 ↓
Order Status
 ↓
LLM
 ↓
Response
```

------------------------------------------------------------------------

# 8. AI Agents ⭐⭐⭐⭐⭐⭐⭐

For senior GenAI roles, this is mandatory.

## LLM vs Agent

### Simple LLM

``` text
User
 ↓
LLM
 ↓
Answer
```

### Agent

``` text
User
 ↓
Agent
 ↓
Reason
 ↓
Plan
 ↓
Tool
 ↓
Observe
 ↓
Reason
 ↓
Tool
 ↓
Final Answer
```

## Learn

-   Agent architecture
-   ReAct
-   Planning
-   Tool use
-   Memory
-   State
-   Reflection
-   Retry
-   Human-in-the-loop
-   Guardrails
-   Agent routing
-   Agent termination
-   Agent evaluation

------------------------------------------------------------------------

# 9. Multi-Agent Systems ⭐⭐⭐⭐⭐

## Supervisor Architecture

``` text
                    User
                      ↓
                Supervisor Agent
                 /      |       \
                ↓       ↓        ↓
          Researcher  Coder   Analyst
                ↓       ↓        ↓
                 \      |      /
                  ↓     ↓     ↓
                  Supervisor
                      ↓
                  Final Answer
```

## Learn

-   Supervisor pattern
-   Sequential agents
-   Parallel agents
-   Hierarchical agents
-   Agent handoff
-   Shared state
-   Agent memory
-   Communication
-   Failure handling

------------------------------------------------------------------------

# 10. LangChain / LangGraph ⭐⭐⭐⭐⭐

## LangChain

Understand:

-   Chains
-   Retrievers
-   Tools
-   Agents
-   Prompt templates
-   Output parsers
-   Document loaders

## LangGraph

Learn deeply:

``` text
State
 ↓
Node
 ↓
Edge
 ↓
Conditional Edge
 ↓
Loop
 ↓
Checkpoint
 ↓
Human Approval
```

## Important LangGraph concepts

-   State
-   Nodes
-   Edges
-   Conditional routing
-   Loops
-   Checkpointing
-   Persistence
-   Human-in-the-loop
-   Multi-agent orchestration
-   Fault tolerance

### Interview question

> Why use LangGraph instead of a simple LangChain chain?

Expected concepts:

-   Stateful workflows
-   Conditional routing
-   Loops
-   Persistence
-   Human-in-the-loop
-   Multi-agent orchestration
-   Fault tolerance

------------------------------------------------------------------------

# 11. MCP --- Model Context Protocol ⭐⭐⭐⭐⭐

MCP is important for modern AI application architecture.

``` text
AI Application
      ↓
     MCP
   /  |  \
  /   |   \
Tools Resources Prompts
```

## Learn

-   MCP client
-   MCP server
-   Tools
-   Resources
-   Prompts
-   Tool discovery
-   Tool invocation
-   MCP architecture
-   Security considerations

### Interview question

> Why do we need MCP when we already have function calling?

Be prepared to explain the difference between a standard tool/function
integration and a reusable protocol/ecosystem for connecting AI
applications with external capabilities and context.

------------------------------------------------------------------------

# 12. AI Application Architecture ⭐⭐⭐⭐⭐⭐⭐

For Forward Deployed AI Engineer / AI Architect roles, this is extremely
important.

``` text
                    React / AG-UI
                         ↓
                    API Gateway
                         ↓
                  AI Application
                         ↓
                    LangGraph
                         ↓
              ┌──────────┼──────────┐
              ↓          ↓          ↓
             LLM       Tools       RAG
              ↓          ↓          ↓
          GPT/Claude   APIs      Vector DB
                                  ↓
                              Reranker
```

## Production concerns

-   Authentication
-   Authorization
-   Rate limiting
-   Streaming
-   WebSocket / SSE
-   Session management
-   Conversation state
-   Redis
-   PostgreSQL
-   Vector DB
-   Observability
-   Logging
-   Guardrails
-   Cost management
-   Failure recovery

------------------------------------------------------------------------

# 13. LLM Evaluation ⭐⭐⭐⭐⭐

A major topic for production GenAI.

## RAG evaluation

-   Context relevance
-   Context precision
-   Context recall
-   Faithfulness
-   Answer relevance
-   Groundedness

## General LLM evaluation

-   Accuracy
-   Hallucination
-   Instruction following
-   JSON validity
-   Safety
-   Toxicity
-   Groundedness

## Tools

-   LangSmith
-   Ragas
-   DeepEval
-   Arize Phoenix

------------------------------------------------------------------------

# 14. LLM Observability ⭐⭐⭐⭐⭐

Production GenAI requires observability.

``` text
User
 ↓
LLM Application
 ↓
Trace
 ├── Prompt
 ├── Retrieval
 ├── Tool Call
 ├── LLM
 ├── Tokens
 ├── Latency
 └── Cost
```

## Learn

-   Distributed tracing
-   Token usage
-   Latency
-   Cost
-   Errors
-   Retrieval quality
-   Tool failures
-   Prompt/version tracking
-   Evaluation traces

------------------------------------------------------------------------

# 15. Fine-Tuning ⭐⭐⭐⭐

Know the fundamentals.

## Topics

-   Fine-tuning vs RAG
-   Supervised Fine-Tuning (SFT)
-   LoRA
-   QLoRA
-   PEFT
-   Dataset preparation
-   Training
-   Evaluation
-   Catastrophic forgetting
-   When NOT to fine-tune

### Common interview question

> RAG or fine-tuning --- which one would you choose?

Answer based on the actual problem and requirements rather than
automatically selecting one.

------------------------------------------------------------------------

# 16. LLM Inference & Performance ⭐⭐⭐⭐

Important for senior AI engineering roles.

## Learn

-   Quantization
-   4-bit / 8-bit quantization
-   KV cache
-   Batching
-   Continuous batching
-   Streaming
-   Model serving
-   vLLM
-   TensorRT-LLM
-   GPU utilization
-   Throughput
-   Tokens/sec
-   Time to First Token (TTFT)
-   Time per output token
-   Context length

------------------------------------------------------------------------

# 17. GenAI Security ⭐⭐⭐⭐⭐

Enterprise GenAI requires strong security.

## Prompt injection

``` text
User
 ↓
Malicious Prompt
 ↓
LLM
 ↓
Unauthorized Behavior
```

## Learn

-   Prompt injection
-   Indirect prompt injection
-   Jailbreaking
-   Data leakage
-   PII protection
-   Secrets exposure
-   Tool abuse
-   Excessive agency
-   RAG poisoning
-   Access control
-   Output validation
-   Guardrails
-   Least-privilege tool access

------------------------------------------------------------------------

# 18. Memory ⭐⭐⭐⭐

Understand different types of memory.

## Short-term / conversation state

``` text
Conversation
      ↓
Short-term State
```

## Long-term memory

``` text
Persistent User Information
          ↓
Long-term Memory
```

## Learn

-   Conversation memory
-   State
-   Checkpointing
-   Semantic memory
-   Episodic memory
-   Working memory
-   Memory retrieval

------------------------------------------------------------------------

# 19. Multimodal GenAI ⭐⭐⭐

Know the fundamentals.

``` text
Text
Images
Audio
Video
Documents
   ↓
Multimodal Model
   ↓
Understanding / Generation
```

## Learn

-   Vision-language models
-   OCR
-   Image understanding
-   Document understanding
-   Speech-to-text
-   Text-to-speech
-   Multimodal RAG

------------------------------------------------------------------------

# 20. AI System Design ⭐⭐⭐⭐⭐⭐⭐

This is one of the most important areas for senior interviews.

## System 1 --- Enterprise RAG

``` text
React
 ↓
API Gateway
 ↓
AI Service
 ↓
LangGraph
 ↓
Retriever
 ↓
Vector DB
 ↓
Reranker
 ↓
LLM
 ↓
Streaming Response
```

## System 2 --- Customer Support Agent

``` text
User
 ↓
Chat UI
 ↓
Agent
 ├── Knowledge Search
 ├── Order API
 ├── CRM API
 ├── Refund API
 └── Human Escalation
```

## System 3 --- Multi-Agent SDLC

``` text
                User
                  ↓
             Supervisor
          /      |       \
         ↓       ↓        ↓
     Analyst    Coder    Tester
         ↓       ↓        ↓
         └───────┼────────┘
                 ↓
              Reviewer
                 ↓
             Deployment
```

------------------------------------------------------------------------

# 🔥 Priority Order

If time is limited, study in this order:

  Priority     Topic
  ------------ -------------------------
  🔥🔥🔥🔥🔥   Transformers
  🔥🔥🔥🔥🔥   LLM Fundamentals
  🔥🔥🔥🔥🔥   RAG
  🔥🔥🔥🔥🔥   Vector DB / Retrieval
  🔥🔥🔥🔥🔥   AI Agents
  🔥🔥🔥🔥🔥   LangGraph
  🔥🔥🔥🔥🔥   AI System Design
  🔥🔥🔥🔥     Tool Calling
  🔥🔥🔥🔥     MCP
  🔥🔥🔥🔥     LLM Evaluation
  🔥🔥🔥🔥     LLM Security
  🔥🔥🔥🔥     Observability
  🔥🔥🔥       Prompt Engineering
  🔥🔥🔥       Fine-tuning
  🔥🔥🔥       LLM Inference
  🔥🔥         Multimodal
  🔥🔥         Advanced Model Training

------------------------------------------------------------------------

# 🧠 Complete GenAI Mental Model

Connect all the concepts together:

``` text
                    GEN AI
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
     LLM            Retrieval         Agents
       │               │                │
 Transformer        Embedding       Tool Calling
 Attention          Vector DB           │
 Tokens             Hybrid Search       MCP
 Context            Reranking           │
 Sampling               │           LangGraph
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                AI Application
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
   Evaluation      Security       Observability
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                Production AI
```

------------------------------------------------------------------------

# 📚 Recommended 8-Module Study Structure

## Module 1 --- Transformer + LLM Fundamentals

Learn:

-   Tokenization
-   Embeddings
-   Attention
-   Multi-head attention
-   Positional encoding
-   Transformer architecture
-   Decoder-only architecture
-   Sampling
-   KV cache

------------------------------------------------------------------------

## Module 2 --- Embeddings + Vector DB + Retrieval

Learn:

-   Embeddings
-   Similarity
-   KNN
-   ANN
-   HNSW
-   Vector databases
-   Dense retrieval
-   Sparse retrieval
-   BM25
-   Hybrid search
-   Metadata filtering

------------------------------------------------------------------------

## Module 3 --- RAG: Basic → Advanced

Learn:

-   Document ingestion
-   Chunking
-   Embeddings
-   Retrieval
-   Reranking
-   Query rewriting
-   Multi-query retrieval
-   HyDE
-   Context compression
-   RAG evaluation

------------------------------------------------------------------------

## Module 4 --- Prompting + Structured Outputs + Tool Calling

Learn:

-   Prompt engineering
-   System prompts
-   Few-shot prompting
-   Structured output
-   JSON schemas
-   Function calling
-   Tool calling
-   Tool errors
-   Tool permissions

------------------------------------------------------------------------

## Module 5 --- Agents + LangGraph + MCP

Learn:

-   Agents
-   ReAct
-   Planning
-   Tool use
-   State
-   LangGraph
-   Conditional routing
-   Loops
-   Checkpointing
-   Human-in-the-loop
-   MCP

------------------------------------------------------------------------

## Module 6 --- Multi-Agent Systems + Memory

Learn:

-   Supervisor architecture
-   Agent handoffs
-   Parallel agents
-   Sequential agents
-   Shared state
-   Agent memory
-   Short-term memory
-   Long-term memory
-   Failure handling

------------------------------------------------------------------------

## Module 7 --- Evaluation + Security + Observability

Learn:

-   RAG evaluation
-   LLM evaluation
-   Hallucination detection
-   Faithfulness
-   Prompt injection
-   RAG poisoning
-   Data leakage
-   Guardrails
-   Tracing
-   Token monitoring
-   Cost monitoring
-   Latency monitoring

------------------------------------------------------------------------

## Module 8 --- End-to-End GenAI System Design + Coding

Practice designing:

1.  Enterprise RAG
2.  Customer Support Agent
3.  Multi-Agent SDLC platform
4.  AI coding assistant
5.  Enterprise knowledge assistant
6.  Document intelligence system
7.  AI workflow automation platform
8.  Agentic search system

------------------------------------------------------------------------


# 21. AI Governance ⭐⭐⭐⭐⭐⭐⭐

AI Governance is especially important for **enterprise GenAI, regulated industries, and senior AI engineering / architecture roles**.

## What is AI Governance?

AI Governance is the set of policies, processes, controls, responsibilities, and technical mechanisms used to ensure that AI systems are:

- Safe
- Secure
- Responsible
- Explainable where required
- Compliant with applicable laws and policies
- Auditable
- Reliable
- Fair
- Privacy-preserving
- Properly monitored throughout their lifecycle

## Core AI Governance Areas

```text
                    AI GOVERNANCE
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
   Responsible AI      Security        Compliance
       │                 │                 │
       ↓                 ↓                 ↓
   Fairness          Privacy          Regulations
   Explainability    Data Security    Policies
   Transparency      Access Control    Audit
       │                 │                 │
       └─────────────────┼─────────────────┘
                         ↓
                 Model Governance
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
   Risk Mgmt        Monitoring        Lifecycle
       │                 │                 │
       ↓                 ↓                 ↓
   Assessment       Drift/Quality      Versioning
   Controls         Incidents          Retirement
```

---

## 21.1 Responsible AI ⭐⭐⭐⭐⭐

Learn:

- Fairness
- Bias
- Transparency
- Explainability
- Accountability
- Human oversight
- Safety
- Reliability
- Responsible deployment

### Important interview question

> How would you make an enterprise GenAI application responsible and trustworthy?

Discuss:

```text
Data Governance
      ↓
Model Selection
      ↓
Risk Assessment
      ↓
Guardrails
      ↓
Evaluation
      ↓
Human Oversight
      ↓
Monitoring
      ↓
Audit
```

---

## 21.2 AI Risk Management ⭐⭐⭐⭐⭐

Understand the AI risk lifecycle:

```text
Identify Risk
     ↓
Assess Risk
     ↓
Mitigate Risk
     ↓
Validate Controls
     ↓
Monitor
     ↓
Incident Response
     ↓
Review / Improve
```

### Risks to understand

- Hallucination
- Bias
- Privacy leakage
- Data leakage
- Prompt injection
- Jailbreaking
- Toxic output
- Incorrect decisions
- Unauthorized tool execution
- Excessive agency
- Model failure
- Supply-chain risk
- Third-party model risk
- RAG poisoning
- Data quality problems

---

## 21.3 Model Governance ⭐⭐⭐⭐⭐

Learn how models are governed across their lifecycle.

```text
Model Selection
      ↓
Approval
      ↓
Testing
      ↓
Validation
      ↓
Deployment
      ↓
Monitoring
      ↓
Periodic Review
      ↓
Retirement
```

Important concepts:

- Model inventory
- Model ownership
- Model versioning
- Model approval
- Model validation
- Model documentation
- Model cards
- Risk classification
- Model lineage
- Model change management
- Model retirement
- Third-party model governance

---

## 21.4 GenAI / LLM Governance ⭐⭐⭐⭐⭐

Specific GenAI governance topics:

- LLM inventory
- Model/provider selection
- Prompt governance
- Prompt versioning
- Prompt approval
- Model version tracking
- System prompt protection
- Tool permissions
- Agent permissions
- RAG data access
- Grounding requirements
- Output validation
- Human approval
- AI-generated content labeling
- Model usage policies
- Cost controls

### Enterprise GenAI governance flow

```text
User
 ↓
Identity / Authorization
 ↓
AI Gateway
 ↓
Policy Check
 ↓
Prompt / Input Guardrails
 ↓
LLM / Agent
 ↓
Tool Authorization
 ↓
RAG Access Control
 ↓
Output Guardrails
 ↓
Audit Logging
 ↓
User
```

---

# 21.5 Data Governance ⭐⭐⭐⭐⭐

AI systems are heavily dependent on data governance.

Learn:

- Data classification
- Data ownership
- Data lineage
- Data quality
- Data retention
- Data minimization
- PII
- Sensitive data
- Data access control
- Encryption
- Data masking
- Anonymization
- De-identification
- Consent
- Data residency
- Training-data governance

### RAG-specific data governance

```text
Enterprise Documents
       ↓
Classification
       ↓
Access Control
       ↓
PII / Sensitive Data Detection
       ↓
Chunking
       ↓
Embedding
       ↓
Vector DB
       ↓
Metadata-based Authorization
       ↓
Retrieval
```

A critical interview topic:

> How do you prevent a RAG system from returning documents that the user is not authorized to access?

---

# 21.6 Privacy & Security Governance ⭐⭐⭐⭐⭐

Understand:

- Privacy by design
- Least privilege
- Authentication
- Authorization
- RBAC
- ABAC
- Encryption at rest
- Encryption in transit
- Secrets management
- PII detection
- PII masking
- Data loss prevention
- Secure logging
- Audit trails

For LLM applications also understand:

- Prompt injection
- Indirect prompt injection
- Data exfiltration
- Tool abuse
- Agent privilege escalation
- Malicious documents
- RAG poisoning

---

# 21.7 AI Compliance ⭐⭐⭐⭐⭐

Know the concept of regulatory and organizational compliance without memorizing every regulation.

Important areas to study:

- Applicable AI regulations
- Privacy regulations
- Industry-specific regulations
- Internal enterprise AI policies
- Data residency requirements
- Audit requirements
- Documentation requirements
- Risk classification
- Human oversight
- Incident reporting
- Third-party/vendor assessments

### Frameworks worth knowing

- NIST AI Risk Management Framework (AI RMF)
- ISO/IEC 42001 — AI Management System
- ISO/IEC 23894 — AI risk management
- ISO/IEC 27001 — Information security management
- OWASP Top 10 for LLM Applications
- EU AI Act — risk-based AI regulation

> For interviews, focus on the principles and how they translate into engineering controls rather than memorizing legal text.

---

# 21.8 AI Auditability & Explainability ⭐⭐⭐⭐

Enterprise AI systems should be traceable.

Learn:

- Audit logs
- Decision logs
- Prompt logs
- Model version logs
- Retrieval logs
- Tool-call logs
- User identity
- Timestamp
- Input/output traceability
- Data lineage
- Model lineage
- Explainability
- Reproducibility

### Example audit trail

```text
User
 ↓
Request ID
 ↓
User Identity
 ↓
Prompt Version
 ↓
Model Version
 ↓
Retrieved Documents
 ↓
Tool Calls
 ↓
Model Output
 ↓
Guardrail Result
 ↓
Final Response
```

---

# 21.9 AI Guardrails ⭐⭐⭐⭐⭐

Guardrails are technical controls that enforce AI application policies.

## Input Guardrails

- Prompt injection detection
- PII detection
- Malicious input detection
- Topic restrictions
- Content classification

## Model Guardrails

- Allowed model list
- Temperature limits
- Token limits
- Approved system prompts
- Model routing policies

## Tool Guardrails

- Tool allowlists
- Permission checks
- Parameter validation
- Rate limits
- Human approval
- Transaction limits

## Output Guardrails

- PII detection
- Toxicity filtering
- Schema validation
- Grounding checks
- Hallucination checks
- Sensitive information filtering

```text
User Input
    ↓
Input Guardrail
    ↓
LLM / Agent
    ↓
Tool Guardrail
    ↓
Output Guardrail
    ↓
Audit
    ↓
Response
```

---

# 21.10 AI Governance for Agents ⭐⭐⭐⭐⭐⭐⭐

This is particularly important as AI systems become agentic.

An agent can potentially:

```text
Read Data
   ↓
Call APIs
   ↓
Modify Records
   ↓
Send Messages
   ↓
Execute Actions
```

Therefore governance must control **what the agent is allowed to do**.

## Learn

- Agent identity
- Agent permissions
- Tool authorization
- Least privilege
- Action boundaries
- Human approval
- Transaction limits
- Tool allowlists
- Agent-to-agent permissions
- Audit trails
- Agent monitoring
- Kill switches
- Retry limits
- Maximum execution steps

### Governed Agent Architecture

```text
                    User
                      ↓
                Authentication
                      ↓
                Policy Engine
                      ↓
                  AI Agent
                      ↓
              ┌───────┼────────┐
              ↓       ↓        ↓
            Search    API      DB
              ↓       ↓        ↓
              └───────┼────────┘
                      ↓
                Authorization
                      ↓
             Human Approval?
                 /         \
               Yes          No
                ↓            ↓
             Execute      Block
                ↓
             Audit Log
```

---

# 21.11 AI Incident Management ⭐⭐⭐⭐

Learn how organizations handle AI incidents.

Examples:

- Major hallucination
- Data leakage
- Prompt injection attack
- Unauthorized tool execution
- Model outage
- Harmful output
- RAG poisoning
- Privacy incident
- Security incident

## Incident lifecycle

```text
Detect
  ↓
Classify
  ↓
Contain
  ↓
Investigate
  ↓
Remediate
  ↓
Validate
  ↓
Document
  ↓
Prevent Recurrence
```

---

# 21.12 AI Governance Interview Questions

Prepare these questions:

1. What is AI Governance?
2. Why is AI Governance important for enterprise GenAI?
3. What is Responsible AI?
4. How would you manage LLM risks?
5. How do you prevent PII leakage?
6. How do you govern enterprise RAG?
7. How do you ensure users only retrieve authorized documents?
8. How do you govern AI agents?
9. How do you control tool permissions?
10. What is human-in-the-loop?
11. How would you audit an LLM interaction?
12. What should be included in an AI audit log?
13. What is model governance?
14. What is model lineage?
15. What is data lineage?
16. How do you handle model/version changes?
17. How do you handle third-party LLM providers?
18. What is AI risk assessment?
19. What are AI guardrails?
20. How would you respond to an AI security incident?
21. What is NIST AI RMF?
22. What is ISO/IEC 42001?
23. What is the OWASP Top 10 for LLM Applications?
24. How would you design a governed enterprise AI platform?

---

# 21.13 Enterprise AI Governance Architecture

A mature enterprise platform can look like:

```text
                         USER
                           ↓
                    Identity / SSO
                           ↓
                      API Gateway
                           ↓
                    AI Governance
                     /     |      \
                    /      |       \
                   ↓       ↓        ↓
              Policy     Risk     Audit
              Engine     Engine   Logging
                   \       |       /
                    \      |      /
                     ↓     ↓     ↓
                    AI Gateway
                         ↓
                ┌────────┼─────────┐
                ↓        ↓         ↓
               RAG      Agent      LLM
                ↓        ↓         ↓
            Vector DB   Tools   Model Provider
                ↓        ↓         ↓
                └────────┼─────────┘
                         ↓
                  Output Guardrails
                         ↓
                   Evaluation
                         ↓
                    Monitoring
                         ↓
                    Audit Store
```

---

# 🔥 Updated Senior GenAI Priority

For an enterprise / senior GenAI interview, the priority becomes:

```text
1.  Transformer + LLM Fundamentals
2.  RAG
3.  Embeddings + Vector Search
4.  AI Agents
5.  LangGraph
6.  Tool Calling
7.  MCP
8.  AI System Design
9.  Multi-Agent Systems
10. LLM Evaluation
11. AI Security
12. AI Governance
13. Guardrails
14. Observability
15. Data Governance
16. Model Governance
17. AI Compliance
18. Fine-tuning
19. LLM Inference
20. Multimodal AI
```

---

# 🏢 Enterprise GenAI Interview Mental Model

For senior interviews, think about GenAI as five layers:

```text
┌──────────────────────────────────────────┐
│            AI APPLICATION                │
│  Chat UI / API / Workflow / Agent       │
├──────────────────────────────────────────┤
│            AI ORCHESTRATION              │
│  LangGraph / Tools / MCP / Agents       │
├──────────────────────────────────────────┤
│             AI INTELLIGENCE              │
│  LLM / Embeddings / RAG / Fine-tuning   │
├──────────────────────────────────────────┤
│          GOVERNANCE & SECURITY           │
│  Policy / Risk / Guardrails / Privacy   │
├──────────────────────────────────────────┤
│         PLATFORM & OPERATIONS            │
│  Observability / Evaluation / Cost      │
│  Scaling / Deployment / Audit           │
└──────────────────────────────────────────┘
```

This is the mindset to use when answering senior-level GenAI system-design questions.

# 🎯 Senior GenAI Interview Focus

For a senior / 6+ year profile, prioritize:

``` text
RAG
 ↓
Agents
 ↓
LangGraph
 ↓
MCP
 ↓
Tool Calling
 ↓
AI System Design
 ↓
Evaluation
 ↓
Security
 ↓
Observability
 ↓
Production Architecture
```

Do not prepare only framework syntax.

Be able to explain:

> Why this architecture?

> Why this model?

> Why RAG instead of fine-tuning?

> Why vector DB?

> Why hybrid search?

> Why reranking?

> How do you handle hallucination?

> How do you evaluate the system?

> How do you secure tools?

> How do you handle failures?

> How do you reduce latency?

> How do you reduce LLM cost?

> How do you scale the system?

> How do you monitor production?

------------------------------------------------------------------------

# 🚀 Final Interview Preparation Strategy

Your preparation should combine **concepts + coding + system design +
real projects**.

``` text
                 GENAI INTERVIEW
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
   Fundamentals     Coding          System Design
       │               │                │
 Transformers       Python          RAG Architecture
 Attention          APIs            Agent Architecture
 RAG                DSA             Multi-Agent
 Agents             Async           Production Design
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                Production Skills
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Security     Evaluation   Observability
          ↓            ↓            ↓
              Enterprise GenAI
```

## Highest-value topics to master

1.  **Transformer architecture**
2.  **LLM fundamentals**
3.  **Embeddings**
4.  **RAG**
5.  **Vector search**
6.  **Hybrid search**
7.  **Reranking**
8.  **Tool calling**
9.  **AI Agents**
10. **LangGraph**
11. **MCP**
12. **Multi-agent orchestration**
13. **Memory**
14. **LLM evaluation**
15. **GenAI security**
16. **Observability**
17. **LLM inference**
18. **Production AI architecture**
19. **AI system design**
20. **End-to-end project implementation**
