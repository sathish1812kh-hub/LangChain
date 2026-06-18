# The AI & GenAI Engineer Learning Roadmap: From Zero to Production Architect

This document details the optimal, dependency-based learning path to go from a beginner with zero prior knowledge to a professional AI Systems Engineer capable of designing, building, and deploying production-grade LLM applications using **RAG, LangChain, LangGraph, and LangSmith**.

---

## Step 1: Technology Dependency Analysis

### Retrieval-Augmented Generation (RAG)
1.  **Why does this technology exist?** To connect stateless, pre-trained language models with external, private, or real-time document datastores.
2.  **What problem does it solve?** It eliminates factual hallucinations and context-window limitations by retrieving only the most relevant document chunks to ground the model's answer.
3.  **What technologies does it depend on?** Parser frameworks, text splitters, embedding models, and vector database indices.
4.  **What concepts must be understood before learning it?** High-dimensional vector coordinate spaces, cosine similarity math, tokenization, semantic search, and document chunk boundary planning.
5.  **What happens if I skip those prerequisites?** You will treat the retrieval pipeline as a "magic black box," leading to poor search precision, noisy prompts, and high hallucination rates.
6.  **Can this technology be learned independently?** Yes. RAG is an architectural pattern, not a framework. It can be built using basic scripts, raw APIs, and custom database clients.

### LangChain
1.  **Why does this technology exist?** To provide a unified middleware SDK that standardizes the interfaces for prompts, models, retrievers, databases, and parsers.
2.  **What problem does it solve?** It eliminates "wrapper fatigue" and coupled code by allowing developers to swap models or vector stores with minimal refactoring.
3.  **What technologies does it depend on?** Concurrency libraries (`asyncio`), data validation libraries (Pydantic), and standard HTTP client connections.
4.  **What concepts must be understood before learning it?** HTTP headers, JSON payloads, Pydantic data schemas, asynchronous event loops, and linear DAG pipeline flow structures.
5.  **What happens if I skip those prerequisites?** You will struggle to debug nested call tracebacks, face blocked execution loops in async servers, and struggle to parse unstructured model responses.
6.  **Can this technology be learned independently?** No. LangChain makes no sense if you do not already understand the APIs, prompt templates, and database retrievers it wraps.

### LangGraph
1.  **Why does this technology exist?** To orchestrate stateful, cyclic agent workflows that require planning, self-correction, and human-in-the-loop approvals.
2.  **What problem does it solve?** It replaces the rigid, linear DAG structure of LangChain with a state-oriented node-and-edge graph system that natively supports loops.
3.  **What technologies does it depend on?** `networkx` (graph mathematics), `pydantic` (state schema validation), and `langchain_core`.
4.  **What concepts must be understood before learning it?** State machines, graph loops, conditional edges, Pydantic field reducers, and checkpoint database savers.
5.  **What happens if I skip those prerequisites?** You will experience infinite agent execution loops, state mutation bugs, and find it difficult to pause/resume sessions.
6.  **Can this technology be learned independently?** No. It is built strictly on top of state schemas and component chains.

### LangSmith
1.  **Why does this technology exist?** To capture telemetry, log traces, version prompts, and run automated evaluations across non-deterministic LLM applications.
2.  **What problem does it solve?** It replaces print statement debugging with visual execution trees and automates regression testing for prompts and models.
3.  **What technologies does it depend on?** PostgreSQL/ClickHouse timeseries logging backends, background HTTP queues, and client-side callback integrations.
4.  **What concepts must be understood before learning it?** Distributed tracing, parent-child span relations, dataset-based evaluations, and feedback loops.
5.  **What happens if I skip those prerequisites?** You will trace code without knowing what metrics to optimize, run evaluations that cost massive tokens unnecessarily, and fail to catch regressions.
6.  **Can this technology be learned independently?** No. Telemetry requires an active, running application to capture.

---

### Dependency Trees

```
Python OOP & Async
│
├── Web APIs & JSON Schema Parsing
│   │
│   └── LLM API Fundamentals & Tokens
│       │
│       └── Vector Embeddings & Similarity Math
│           │
│           └── Vector Databases & Indexing
│               │
│               └── Retrieval-Augmented Generation (RAG)
│                   │
│                   └── LangChain (LCEL Orchestration)
│                       │
│                       └── LangGraph (Cyclic Agents)
│                           │
│                           └── LangSmith (Observability & Evaluation)
```

---

## Step 2: Beginner Learning Roadmap

---

### Stage 1: Python Foundations
*   **Why this stage exists:** Python is the programming standard for the AI/GenAI ecosystem.
*   **Topics to learn:** Data types, loops, lists, dictionaries, OOP principles, and asynchronous execution (`asyncio`).
*   **Concepts to master:** Event loops, coroutines, type hinting, and Pydantic validation structures.
*   **Skills gained:** Writing clean concurrent scripts, validating data shapes, and handling exceptions.
*   **Common mistakes:** Skipping asynchronous programming (`async/await`) and writing purely synchronous blocking scripts.
*   **Completion criteria:** Write an async script that calls multiple APIs in parallel, parses outputs, and handles failures.

---

### Stage 2: APIs and JSON
*   **Why this stage exists:** LLMs and database services communicate via JSON payloads over HTTP.
*   **Topics to learn:** REST standards, HTTP headers, API status codes, authentication, and parsing JSON strings.
*   **Concepts to master:** JSON Schemas, rate-limiting, backoff strategies, and connection timeouts.
*   **Skills gained:** Building robust API client connections and formatting structured requests.
*   **Common mistakes:** Hardcoding API keys in code files and failing to write error-handling code for network timeouts.
*   **Completion criteria:** Write a script that executes authenticated HTTP POST requests, validates the response headers, and handles status code failures.

---

### Stage 3: LLM Fundamentals
*   **Why this stage exists:** To understand the constraints, capabilities, and token metrics of the core reasoning engines.
*   **Topics to learn:** Text tokens, context windows, API billing parameters (temperature, top_p, max_tokens), and model roles.
*   **Concepts to master:** Context limit constraints, role formatting (system, user, assistant), and non-deterministic behavior.
*   **Skills gained:** Selecting model checkpoints, calculating token usage budgets, and managing model configurations.
*   **Common mistakes:** Assuming LLMs act as structured databases and setting temperature to high values during structured parsing tasks.
*   **Completion criteria:** Write a script that uses raw SDK calls to prompt a model, validates the token costs, and handles context window errors.

---

### Stage 4: Prompt Engineering
*   **Why this stage exists:** Prompting is the primary method to configure instructions and extract structured outputs from models.
*   **Topics to learn:** Zero-shot prompting, few-shot prompting, Chain-of-Thought (CoT), XML tagging, and system rules.
*   **Concepts to master:** Instruction formatting, prompt injection defense, and schema enforcement techniques.
*   **Skills gained:** Formatting reliable instruction sets and extracting clean data structures.
*   **Common mistakes:** Writing conversational prompts without explicit role tags and relying on unstructured text descriptions.
*   **Completion criteria:** Write a system prompt template that extracts a JSON array containing data fields from unstructured logs.

---

### Stage 5: Embeddings
*   **Why this stage exists:** To convert natural language concepts into numerical vectors for search operations.
*   **Topics to learn:** Vector representations, high-dimensional spaces, embedding dimensions, and distance calculations.
*   **Concepts to master:** Vector cosine similarity, dot product calculation, and embedding model capabilities.
*   **Skills gained:** Vectorizing text strings and mathematically calculating semantic similarity between strings.
*   **Common mistakes:** Mixing different embedding models in a single index and assuming embeddings are ideal for keyword lookups.
*   **Completion criteria:** Convert three different sentences into embedding vectors and calculate their cosine similarity scores.

---

### Stage 6: Vector Search
*   **Why this stage exists:** Traditional databases are not optimized for fast nearest-neighbor lookups over millions of vectors.
*   **Topics to learn:** Vector similarity comparisons, inverted indexes, and BM25 lexical search.
*   **Concepts to master:** Keyword precision vs. Semantic matching, and Reciprocal Rank Fusion (RRF).
*   **Skills gained:** Performing nearest-neighbor searches and merging lexical and semantic search results.
*   **Common mistakes:** Confusing exact character matching (product codes) with semantic similarity.
*   **Completion criteria:** Build a hybrid search script that retrieves and combines exact matching keywords and semantic concepts.

---

### Stage 7: Vector Databases
*   **Why this stage exists:** To scale, index, and query millions of vectors efficiently.
*   **Topics to learn:** Vector indexes (HNSW, IVF), metadata indexing, payload filters, and database schemas.
*   **Concepts to master:** Approximate Nearest Neighbor (ANN) search and metadata index filtering.
*   **Skills gained:** Initializing database clients, writing index schemas, and executing queries with metadata filters.
*   **Common mistakes:** Using Chroma for production deployment instead of Qdrant or Pinecone.
*   **Completion criteria:** Deploy a local Qdrant instance, index 100 document vectors with metadata keys, and execute a filtered search.

---

### Stage 8: RAG
*   **Why this stage exists:** RAG is the production standard to ground LLMs in private documents without retraining costs.
*   **Topics to learn:** Document loaders, parsing tables, recursive chunking, reranking models, and prompt assembly.
*   **Concepts to master:** Ingestion pipelines, context pollution limits, and the retrieve-then-read lifecycle.
*   **Skills gained:** Building end-to-end retrieval pipelines, optimizing search recall, and generating grounded responses.
*   **Common mistakes:** Splitting chunks by raw character counts without sentence boundaries, and failing to use reranking.
*   **Completion criteria:** Build a RAG script that parses a PDF file, splits chunks recursively, indexes them, retrieves context, and generates answers.

---

### Stage 9: LangChain
*   **Why this stage exists:** To replace custom wrapper code with standard component interfaces and compiled chains.
*   **Topics to learn:** LCEL pipe operator (`|`), `Runnable` protocol, `ChatPromptTemplate`, and `PydanticOutputParser`.
*   **Concepts to master:** Declarative composition, async execution routing, and output validation schemas.
*   **Skills gained:** Composing multi-stage pipelines, switching models, and streaming tokens.
*   **Common mistakes:** Using deprecated `LLM` classes instead of `ChatModel`, and writing bloated LCEL chains for simple prompts.
*   **Completion criteria:** Build an LCEL chain that takes a user query, retrieves context, calls a model, and parses outputs to a Pydantic schema.

---

### Stage 10: LangGraph
*   **Why this stage exists:** To support stateful agent loops, self-correction, and human validation checkpoints.
*   **Topics to learn:** `StateGraph`, Nodes, Edges, Conditional Edges, Reducers, Checkpoint Savers, and Interrupts.
*   **Concepts to master:** State-oriented execution graphs, cyclical loops, and human-in-the-loop breakpoints.
*   **Skills gained:** Building multi-agent systems, writing error recovery loops, and pausing execution graphs.
*   **Completion criteria:** Build an agent that runs tools, checks outputs for errors, loops to self-correct, and pauses for approval on sensitive operations.

---

### Stage 11: LangSmith
*   **Why this stage exists:** To debug, monitor, version, and evaluate LLM applications in production.
*   **Topics to learn:** Tracing environment configurations, parent-child span relations, datasets, and run evaluators.
*   **Concepts to master:** Distributed telemetry metrics, prompt version hub, and LLM-as-a-Judge evaluations.
*   **Skills gained:** Debugging nested chain tracebacks, versioning prompts, and running automated regression testing.
*   **Common mistakes:** Tracing local test loops, and running expensive evaluator models on every production run.
*   **Completion criteria:** Run an automated evaluation test against a dataset of 50 examples and analyze the faithfulness score.

---

### Stage 12: Production AI Systems
*   **Why this stage exists:** To containerize, host, scale, and secure LLM applications for real-world traffic.
*   **Topics to learn:** Containerization (Docker), API routers (FastAPI), secure access controls, rate limiting, and caching.
*   **Concepts to master:** Multi-tenant security isolation, API performance scale, and CI/CD testing pipelines.
*   **Skills gained:** Containerizing graphs, configuring API endpoints, setting up rate limits, and implementing access filters.
*   **Common mistakes:** Exposing raw admin database ports and deploy without telemetry logging or token cost controls.
*   **Completion criteria:** Deploy a secure, containerized API running a stateful search agent, with Postgres checkpoint persistence.

---

## Step 3: Readiness Validation

### Stage 1: Python Foundations
*   *Explain:* The execution difference between synchronous and asynchronous code.
*   *Build:* A script that executes multiple concurrent tasks using `asyncio.gather`.
*   *Troubleshoot:* `Coroutine was never awaited` errors and blocked event loops.
*   *Concept:* Asynchronous coroutines and Pydantic data schemas.

### Stage 2: APIs and JSON
*   *Explain:* HTTP status code groups and how rate limits affect integrations.
*   *Build:* An API client wrapper that executes POST queries with retry backoff configurations.
*   *Troubleshoot:* Connection timeouts, authorization failures, and malformed JSON payloads.
*   *Concept:* JSON Schema structures and REST API design patterns.

### Stage 3: LLM Fundamentals
*   *Explain:* What tokens are, how they map to text, and context window limitations.
*   *Build:* A raw SDK prompt caller that tracks input/output tokens and cost metrics.
*   *Troubleshoot:* Token limit errors, non-deterministic formatting changes, and hyperparameter errors.
*   *Concept:* Difference between system, user, and assistant roles.

### Stage 4: Prompt Engineering
*   *Explain:* Zero-shot vs. Few-shot formatting, and instruction structuring.
*   *Build:* A system prompt that enforces strict JSON formatting using XML tags.
*   *Troubleshoot:* Prompt injection bypasses and unstructured output text errors.
*   *Concept:* Context isolation and structured prompt templates.

### Stage 5: Embeddings
*   *Explain:* How semantic distance is calculated mathematically using cosine similarity.
*   *Build:* A script that embeds a list of sentences and logs the most matching pairs.
*   *Troubleshoot:* Out-of-bounds vector dimensions and model mismatch errors.
*   *Concept:* Coordinate representation and semantic vector space.

### Stage 6: Vector Search
*   *Explain:* The limitations of semantic search vs. keyword lookup.
*   *Build:* A Reciprocal Rank Fusion (RRF) ranker that merges keyword and vector matches.
*   *Troubleshoot:* Missing exact keyword matches and poor semantic matches.
*   *Concept:* Semantic concepts vs. Lexical exact characters.

### Stage 7: Vector Databases
*   *Explain:* Approximate Nearest Neighbor (ANN) search and HNSW indexing.
*   *Build:* A Qdrant indexing script that uploads payloads and queries with metadata filters.
*   *Troubleshoot:* Slow query times, indexing errors, and missing metadata filters.
*   *Concept:* Vector indices, payloads, and partition splits.

### Stage 8: RAG
*   *Explain:* Why chunk overlap is required, and what Reranking does.
*   *Build:* A RAG pipeline with recursive chunking, vector retrieval, reranking, and prompt grounding.
*   *Troubleshoot:* Factual hallucinations, lost-in-the-middle context issues, and duplicate chunks.
*   *Concept:* Ingestion pipelines and context grounding.

### Stage 9: LangChain
*   *Explain:* The `Runnable` interface and LCEL pipeline composition.
*   *Build:* An LCEL chain with a retriever, prompt, chat model, and Pydantic parser.
*   *Troubleshoot:* Nested chain exceptions and parsing failures.
*   *Concept:* Declarative routing pipelines.

### Stage 10: LangGraph
*   *Explain:* Nodes, Edges, Reducers, and checkpointer persistence.
*   *Build:* A tool-using state graph agent that self-corrects on errors.
*   *Troubleshoot:* Infinite loops, lost state changes, and thread state corruption.
*   *Concept:* Stateful state machines and cyclical execution loops.

### Stage 11: LangSmith
*   *Explain:* Traces, spans, runs, and dataset evaluations.
*   *Build:* An automated test script that evaluates a pipeline against a dataset.
*   *Troubleshoot:* Telemetry latency, missing traces, and costly evaluations.
*   *Concept:* Distributed tracing and semantic model judges.

### Stage 12: Production AI Systems
*   *Explain:* Multi-tenant access controls, container hosting, and deployment scaling.
*   *Build:* A containerized FastAPI app hosting a state graph agent with Postgres persistence.
*   *Troubleshoot:* Database locks, deployment crashes, and security permission leaks.
*   *Concept:* Production architecture scaling.

---

## Step 4: Learning Priority

| Rank | Topic/Technology | Why It Matters | Difficulty | Learning Priority | Production Importance |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **1** | Python Async & Pydantic | Base runtime for concurrent pipelines and validation schemas. | Medium | Critical | Critical |
| **2** | REST APIs & JSON | Base protocol to interface with models and databases. | Low | Critical | Critical |
| **3** | LLM API Core | Understands token budgets, roles, and context limits. | Low | Critical | Critical |
| **4** | Recursive Chunking & Overlap | Key step in parsing documents without losing semantic context. | Medium | High | High |
| **5** | Hybrid Search (BM25 + Vector) | Essential to match both exact terms and semantic concepts. | Medium-High| High | High |
| **6** | Reranking (Cross-Encoder) | Filters out retrieval noise, reducing prompt footprint. | Low | High | High |
| **7** | LangChain LCEL & Parsers | Replaces boilerplate integrations with standard schemas. | Medium | High | Medium-High |
| **8** | LangSmith Tracing & Telemetry | Essential to audit execution steps and debug failures. | Low | Critical | Critical |
| **9** | LangGraph Stateful Loops | Enables cyclic agents, self-correction, and pauses. | High | High | High |
| **10** | PostgresSaver Checkpointer | Saves session states securely across server restarts. | Medium | Medium | Critical |
| **11** | Dataset Evaluations (Ragas) | Automates regression testing for updates. | Medium-High| Medium | High |
| **12** | Multi-tenant Access Controls | Enforces data security and permission boundaries. | High | Medium | Critical |

---

## Step 5: Common Mistakes

### Mistake 1: Learning LangChain before RAG
*   **Why it happens:** Beginners follow tutorials that use LangChain immediately, without explaining the underlying concepts.
*   **Consequences:** You treat RAG as a "magic function" of LangChain, making it impossible to tune splitters, debug database queries, or resolve retrieval issues.
*   **Correct Approach:** Build a manual RAG pipeline using raw Python, file loaders, and database clients first, then swap it for LangChain abstractions once the concepts are clear.

### Mistake 2: Learning LangGraph before LangChain
*   **Why it happens:** Developers want to build complex multi-agent systems immediately.
*   **Consequences:** You struggle to write nodes and edges because you do not understand prompt templates, models, parsers, or the basic `Runnable` interface.
*   **Correct Approach:** Master LangChain components (LCEL, Prompts, ChatModels, OutputParsers) first, then use them as nodes inside a LangGraph StateGraph.

### Mistake 3: Learning LangSmith before building AI systems
*   **Why it happens:** Developers try to learn tracing and evaluation platforms without an active project.
*   **Consequences:** Telemetry dashboards remain empty, and you cannot understand spans, runs, or evaluations.
*   **Correct Approach:** Build and run local RAG or agent applications first, then enable environment variables to log traces to LangSmith.

### Mistake 4: Memorizing code without understanding concepts
*   **Why it happens:** Tutorial videos encourage copy-pasting template code.
*   **Consequences:** When models update or database schemas change, the application breaks and you cannot locate the failure in the stack trace.
*   **Correct Approach:** Focus on the conceptual pipeline (load -> split -> embed -> index -> query -> rerank -> prompt -> generate) before writing code.

### Mistake 5: Skipping embeddings and vector databases
*   **Why it happens:** Developers use simple character splitters and text searches to get quick results.
*   **Consequences:** Search recall is poor, synonym matching fails, and the application cannot scale to large document libraries.
*   **Correct Approach:** Study vector spaces, coordinate math, similarity distance, and index structures before building databases.

---

## Step 6: Final Answer

### Complete Dependency Tree
```
1. Python async/await & OOP
   └── 2. Web APIs & JSON Payload parsing
       └── 3. LLM API parameters & roles
           └── 4. Zero-shot & Few-shot Prompting
               └── 5. Embeddings & Cosine Similarity math
                   └── 6. Lexical vs. Semantic Vector Search
                       └── 7. HNSW Indexing & Vector DBs (Qdrant)
                           └── 8. RAG Ingestion & Reranking
                               └── 9. LangChain LCEL & Parsers
                                   └── 10. LangGraph Nodes & Edges
                                       └── 11. LangSmith Tracing & Evaluators
                                           └── 12. FastAPI, Docker & Postgres Saver
```

---

### Technology Relationship Diagram

```
+-------------------------------------------------------------------------------+
|                                FastAPI Server                                 |
|  - Validates user sessions and permissions                                    |
|  - Exposes REST / Streaming WebSocket routes                                  |
|  - Executes compiled LangGraph state machines                                 |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
|                           Orchestration: LangGraph                            |
|  - Shared State Schema (Validated via Pydantic)                               |
|  - Nodes: LangChain Prompt/Model Chains                                       |
|  - Edges: Router logic based on tool outputs                                  |
+-------------------+-------------------+-------------------+-------------------+
                    |                   |                   |
                    v Vector Search     v API Calls         v Telemetry
+-------------------+---+ +-------------+-----+ +-----------+---+ +-------------+
| Vector database       | | LLM Providers     | | Postgres DB   | | LangSmith   |
| (Qdrant hybrid index) | | (Claude / Gemini) | | (Checkpoints) | | (Tracing)   |
+-----------------------+ +-------------------+ +---------------+ +-------------+
```

---

### Study Order & Estimated Time

| Stage | Focus | Primary Concept | Estimated Time | Start Condition | Stop Condition |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **1** | Python Foundations | Coroutines & Schemas | 10 Hours | Zero knowledge. | Can run async gathers and validate data schemas. |
| **2** | APIs and JSON | HTTP Request Lifecycle | 6 Hours | Stage 1 done. | Can configure async post clients with retries. |
| **3** | LLM API Core | Token budgets & Roles | 4 Hours | Stage 2 done. | Can calculate token costs from API outputs. |
| **4** | Prompt Engineering | Instruction limits | 6 Hours | Stage 3 done. | Extract validated JSON shapes via prompts. |
| **5** | Embeddings | Vector Space | 6 Hours | Stage 4 done. | Calculate similarity scores of sentences. |
| **6** | Vector Search | Exact vs. Semantic | 8 Hours | Stage 5 done. | Build a functional hybrid search script. |
| **7** | Vector Databases | HNSW Indexing | 10 Hours | Stage 6 done. | Run local database searches with filters. |
| **8** | Ingestion & RAG | Context optimization | 12 Hours | Stage 7 done. | Build a RAG script with splitter and reranker. |
| **9** | LangChain Core | Declarative LCEL | 15 Hours | Stage 8 done. | Build an LCEL chain with database and parser. |
| **10** | LangGraph Orchestration| Stateful Cycles | 20 Hours | Stage 9 done. | Build a tool-using agent that self-corrects. |
| **11** | LangSmith Telemetry | Distributed Tracing | 10 Hours | Stage 10 done. | Run regression tests on datasets. |
| **12** | Production Operations | Containerized APIs | 15 Hours | Stage 11 done. | Deploy a secure API with postgres persistence. |

---

### Top 20% Concepts (80% of AI Engineering Value)
1.  **Asynchronous I/O Execution:** Crucial to keep servers responsive during slow model and tool requests.
2.  **Pydantic Schema Validation:** Standardizes the formatting of unstructured model text into verified schemas.
3.  **Hybrid Search Indexing:** Combines keyword precision and semantic matches to maximize retrieval quality.
4.  **Reranking (Cross-Encoder):** Filters out noisy database matches, reducing prompt sizes and token costs.
5.  **Stateful checkpointer loops:** Saves session state checkpoints, allowing agents to run loops and recover from failures.

---

### Start & Stop Conditions

#### 📍 Start Today:
Write a simple Python script using `asyncio` that runs three dummy helper tasks concurrently. Confirm that tasks run in parallel, and learn how to use Pydantic classes to validate data inputs.

#### 🛑 Stop and Move On:
Do not move to the next stage until you can build the target project for that level from scratch without copying template code, and explain how the underlying concepts function (e.g. explain HNSW indexing before using cloud vector databases).

---

### Career Progression Roadmap

```
[Beginner]
   │
   ├── Concepts: Python, APIs, JSON parsing
   ├── Skills: Writing scripts, handling APIs
   └── Target: Automated scripting
   ▼
[AI Engineer]
   │
   ├── Concepts: Prompting, Embeddings, Hybrid Search
   ├── Skills: Building RAG search pipelines, calling model APIs
   └── Target: Grounded Q&A Search Engines
   ▼
[Senior AI Engineer]
   │
   ├── Concepts: LCEL chains, State graphs, Tool calling
   ├── Skills: Stateful agents, self-correction, tracing logs
   └── Target: Autonomous Code & Task Agents
   ▼
[AI Platform Architect]
   │
   ├── Concepts: Multi-tenant database security, Postgres savers, Docker
   ├── Skills: Secure corporate integrations, scaling cluster containers
   └── Target: Secure Enterprise Knowledge Portals
```
