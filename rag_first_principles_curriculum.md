# RAG Core Curriculum: First-Principles Lessons (Beginner to Advanced)

This document contains the complete, progressive curriculum designed to teach **Embeddings, Vector Search, Vector Databases, and Retrieval-Augmented Generation (RAG)**. Each phase includes both a conceptual explanation for beginners and a technical breakdown for advanced engineers, followed by comprehension quizzes.

---

## Phase 1: Embeddings

```
+--------------------------------------------------------------+
|                     Beginner Metaphor                        |
| Think of a library where books with similar topics are placed |
| on the same shelf. The coordinates of the shelf represent the |
| semantic meaning of the book.                                |
+--------------------------------------------------------------+
                                |
                                v
+--------------------------------------------------------------+
|                     Advanced Architecture                    |
| Text -> Sub-word Tokenizer -> Transformer Encoder Layers     |
| -> Contextual Hidden States -> Mean Pooling -> Vector Array  |
+--------------------------------------------------------------+
```

### 1. Concept Deconstruction
*   **Why it exists:** Computers do not understand words or concepts. They only process math. Standard databases treat words as isolated strings, failing to recognize that "car" and "automobile" are related.
*   **What problem it solves:** Converts unstructured text into a dense array of floating-point numbers (a vector) that mathematically represents its semantic meaning.
*   **How it works:**
    *   *Beginner:* Think of a massive coordinate system. The word `"apple"` is placed at `[x, y, z]` near `"banana"` (fruit) and `"Microsoft"` (tech company).
    *   *Advanced:* The input text is mapped to a vector space of $D$ dimensions (usually 768 or 1536) using a neural network. The location in this vector space represents the semantic features of the text.
*   **Advantages:** Captures semantic meaning, resolves synonym search problems, and reduces language to coordinate geometry.
*   **Disadvantages:** Processing high-dimensional arrays is computationally expensive, and models can lose precision on long documents.
*   **Real-World Example:** Mapping `"automobile"` and `"car"` to nearby vectors: `[0.23, -0.45, 0.89]` vs. `[0.24, -0.44, 0.88]`.

### 2. Historical Problems before Embeddings
*   **Why it exists:** Searching and grouping documents mathematically existed long before modern transformer models.
*   **What problem it solves:** Text categorization and search queries.
*   **How it works:**
    *   **One-Hot Encoding:** Represented each word as a sparse binary array containing a single `1` (e.g. `[0, 0, 1, 0, 0]`).
    *   **TF-IDF:** Scored word frequency relative to a document database.
*   **Advantages:** Fast, deterministic, and requires no neural network compute.
*   **Disadvantages:**
    *   *No Semantic Context:* `"Not good"` and `"good"` had no mathematical relationship.
    *   *Dimensionality Explosion:* A vocabulary of 100,000 words required 100,000-dimensional sparse arrays.
    *   *Synonym Blindness:* Searching for `"automobile"` yielded zero matches if the document only used `"car"`.
*   **Real-World Example:** Classic SQL `LIKE '%term%'` lookups or early keyword search engines.

### 3. How Text Becomes Vectors (Tokenizer to Pooling)
*   **Why it exists:** Raw text strings must be converted into numerical values before a neural network can process them.
*   **What problem it solves:** Normalizes character sequences, maps them to token IDs, and resolves context differences.
*   **How it works:**
    1.  **Tokenization:** Splits text into sub-word tokens (e.g. `"embed"` + `"dings"`).
    2.  **Embedding Lookup:** Each token ID retrieves a starting vector from a vocabulary weight table.
    3.  **Attention Processing (Encoder):** A Transformer encoder processes the vectors, adjusting weights based on surrounding words (e.g., identifying `"bank"` as a river bank vs. a financial bank).
    4.  **Pooling:** The final contextual token vectors are averaged (Mean Pooling) or the first token output (`[CLS]` token) is selected to generate a single dense vector representing the entire text input.
*   **Advantages:** Generates context-sensitive vector representations.
*   **Disadvantages:** Constrained context limits (e.g., 8192 tokens max).
*   **Real-World Example:** The word `"running"` is split into `["run", "ning"]` and mapped to a dense array.

### 4. Vector Space Intuition & Semantic Similarity
*   **Why it exists:** To establish a geometric coordinate system where distance represents semantic similarity.
*   **What problem it solves:** Enables vector algebra to be applied to human concepts.
*   **How it works:**
    *   *Beginner:* Imagine a 3D space with axes representing: `[wheels, engine, flies]`.
        *   `"bicycle"` = `[2, 0, 0]`
        *   `"car"` = `[4, 1, 0]`
        *   `"airplane"` = `[3, 1, 1]`
    *   *Advanced:* Models use 1536 dimensions, mapping abstract features (e.g. gender, tense, classification) across high-dimensional hyperplanes.
*   **Advantages:** Enables semantic arithmetic (e.g., Vector(`"King"`) - Vector(`"Man"`) + Vector(`"Woman"`) $\approx$ Vector(`"Queen"`)).
*   **Disadvantages:** High-dimensional spaces are impossible for humans to visualize.
*   **Real-World Example:** Recommendation engines matching products with similar semantic coordinates.

### 5. Cosine Similarity vs. Other Metrics
*   **Why it exists:** We need a formula to calculate the similarity of two high-dimensional vectors.
*   **What problem it solves:** Standard Euclidean distance focuses on text length (vector magnitude); cosine similarity measures the *direction* (angle) of vectors, ignoring length variations.
*   **How it works:**
    *   **Cosine Similarity:** Calculates the cosine of the angle between two vectors:
        $$\text{Cosine Similarity}(A, B) = \frac{A \cdot B}{\|A\| \|B\|}$$
        *   `1.0` = Identical direction (highly similar).
        *   `0.0` = Orthogonal (no relation).
        *   `-1.0` = Opposite directions.
    *   **Dot Product:** Cosine similarity scaled by vector magnitude. If vectors are normalized to length `1.0`, Dot Product is identical to Cosine Similarity.
    *   **Euclidean Distance:** Measures the straight-line distance between coordinate points.
*   **Advantages:** Cosine similarity handles varying document lengths accurately.
*   **Disadvantages:** Calculating similarity across millions of vectors sequentially is slow (requires indexing).
*   **Real-World Example:** Matching a short query `"need database help"` to a long document section.

---

### Phase 1 Quiz
1. Why does a keyword search for `"automobile"` fail to return a document containing `"car"`, and how do embeddings solve this?
2. Explain the difference between `[CLS]` token pooling and Mean Pooling. What are the trade-offs?
3. If two document vectors have a Euclidean distance of $25.0$ but a Cosine Similarity score of $0.98$, what does this tell you about the relationship between the two documents?
4. Why is it mathematically impossible to compare vector embeddings generated by OpenAI's `text-embedding-3-small` (1536 dimensions) with Google's `Gecko` model (768 dimensions)?
5. What does a Cosine Similarity score of $0.0$ represent?

---

## Phase 2: Vector Search

```
Keyword Search (Exact match)
  Query: "blue shoes" ---> [BM25 Index] ---> Matches: "blue shoes"

Vector Search (Semantic match)
  Query: "navy sneakers" ---> [Embedding Model] ---> [Vector Index] ---> Matches: "blue shoes"
```

### 1. What and Why
*   **Historical Problem:** Sequential search comparison (KNN) requires scanning every vector in the database, resulting in high latency as datasets scale.
*   **Purpose:** To locate vectors closest in meaning to a query vector within milliseconds.
*   **Internal Workflow:** Query string is embedded -> Nearest neighbor search navigates the index -> Returns top matching document nodes.
*   **Advantages:** Extremely fast semantic lookups, scalable search.
*   **Limitations:** Struggles with exact search queries (like model numbers or names).
*   **Production Use Cases:** Document search portals, e-commerce catalog search.

### 2. Approximate Nearest Neighbor (ANN)
*   *Why:* Trades a tiny bit of search accuracy for massive speed improvements.
*   *HNSW (Hierarchical Navigable Small World):* A graph-based index where top layers route search queries quickly, and bottom layers locate precise local matches.
*   *IVF (Inverted File Index):* Clusters vectors into groups, allowing queries to search only the most relevant cluster, skipping the rest of the database.

---

### Phase 2 Quiz
1. Explain the performance difference between KNN and ANN search.
2. How does the HNSW graph index navigate to nearest vectors?
3. What is the key trade-off when using IVF indexing?
4. Explain how Reciprocal Rank Fusion (RRF) combines keyword and semantic search results.
5. In what scenarios does vector search fail compared to lexical BM25 search?

---

## Phase 3: Vector Databases

```
+-------------------------------------------------------------+
|                     Vector Database                         |
|  - Stores vectors + raw text payloads                       |
|  - Pre-computed HNSW Graph Indices                          |
|  - Metadata single-stage filtering engine                   |
+-------------------------------------------------------------+
```

### 1. Why SQL is Insufficient
*   *B-Tree Limitations:* SQL indexes (B-trees) sort scalar data in one dimension, which fails to navigate multi-dimensional coordinate spaces.
*   *Compute Overhead:* Running similarity calculations across millions of rows in a relational database degrades query performance.

### 2. Indexing, Storage, and Retrieval
*   *Vector Storage:* Stores high-dimensional floating-point arrays alongside raw document payloads.
*   *Metadata Filtering:*
    *   *Pre-filtering:* Filters results by metadata first, then searches vectors. Can lead to poor results if the filter matches too few items.
    *   *Post-filtering:* Searches vector space first, then filters results by metadata. Risk of returning less than the requested number of results if matches are filtered out.
    *   *Single-Stage:* Filters metadata dynamically during HNSW graph traversal. The standard for production systems.

### 3. Database Platforms Comparison
*   **Qdrant:** Rust performance, hybrid search, payload filtering. Excellent for production.
*   **Pinecone:** Serverless setup, auto-scaling, zero ops. SaaS-only.
*   **Weaviate:** Go-based object-vector hybrid database.
*   **pgvector:** PostgreSQL extension. Easy integration with existing databases.
*   **Chroma:** Local in-memory store. Good for local testing, poor for production scaling.

---

### Phase 3 Quiz
1. Why do SQL B-tree indexes fail on vector searches?
2. What is the difference between Pre-filtering, Post-filtering, and Single-Stage filtering?
3. When should pgvector be chosen over a dedicated vector database like Qdrant?
4. What are the main limitations of Chroma in production?
5. Why does Pinecone introduce vendor lock-in risk?

---

## Phase 4: Retrieval-Augmented Generation (RAG)

```
                       +-------------------+
                       |    User Query     |
                       +---------+---------+
                                 |
                                 v
                       +---------+---------+
                       | Hybrid Search     | <--- BM25 + Vector Index
                       +---------+---------+
                                 | Top matches
                                 v
                       +---------+---------+
                       | Reranker Model    |
                       +---------+---------+
                                 | Top 5 chunks
                                 v
                       +---------+---------+
                       | Context Prompt  | ---> [ LLM Generation ]
                       +-------------------+
```

### 1. Why RAG Exists
*   *Factual Grounding:* Restricts model answers to retrieved context to eliminate hallucinations.
*   *Knowledge Cutoffs:* Allows models to access real-time information without retraining.
*   *Private Data:* Maps private files to prompts without uploading them to model training datasets.

### 2. Ingestion & Retrieval Pipeline
1.  **Ingestion:** Loaders parse files, and splitters chunk text recursively with overlap.
2.  **Indexing:** Chunk embeddings are indexed in the vector database.
3.  **Retrieval:** Hybrid search retrieves matching chunks, and rerankers select top results.
4.  **Generation:** Context is appended to the prompt, and the model generates grounded answers.

### 3. RAG Style Comparison
*   **Naive RAG:** Simple retrieval and generation. Prone to noise.
*   **Advanced RAG:** Naive RAG + Parent-child indexing + Reranking.
*   **Hybrid RAG:** Lexical + Vector retrieval.
*   **Agentic RAG:** Model uses tools to search and evaluate results.
*   **Graph RAG:** Combines vector search with knowledge graphs.

---

### Phase 4 Quiz
1. Explain the retrieve-then-read lifecycle in RAG.
2. Why is chunk overlap necessary during text splitting?
3. How does a Reranker model reduce context window congestion?
4. What is the difference between Naive RAG and Advanced RAG?
5. Explain how a Parent-Child retriever optimizes search precision.

---

## Final Evaluation

### Concept Map
```
                  [Raw Document] -> [Recursive Splitter] -> [Child Vectors]
                                                                  │
                                                                  v Indexed
                                                          [Vector Store]
                                                                  ^
    [User Query] -> [Hybrid Search (BM25 + Vector)] --------------+
                          │
                          v Top results
                  [Reranker Model] -> [Context Prompt] -> [Grounded Answer]
```

### Common Production Mistakes
1.  *Ignoring Ingestion Quality:* Feeding raw headers and formatting noise into databases.
2.  *Skipping Reranking:* Relying on raw vector database scores.
3.  *Missing Access Controls:* Searching databases without permissions filters.
