# Retrieval Augmented Generation (RAG) Specification

## 1. Overview

This document specifies the design and functionality of the Retrieval Augmented Generation (RAG) system within the project. RAG is employed to enhance the capabilities of Language Model (LLM) agents by providing them with relevant, up-to-date, and domain-specific information beyond their initial training data. This system leverages external knowledge bases and sophisticated indexing techniques to retrieve pertinent context, which is then provided to the Gemini LLM for generating more accurate and informed responses.

## 2. Goals

*   **Improve LLM Response Accuracy:** Provide ground truth information to LLMs to reduce hallucination and improve factual accuracy.
*   **Enable Domain-Specific Knowledge:** Allow LLMs to access and utilize knowledge from private or specialized datasets.
*   **Enhance Timeliness of Information:** Incorporate the latest data into LLM responses without retraining the base models.
*   **Support Agent Tooling:** Expose RAG capabilities as tools for autonomous agents to query and integrate external information.
*   **Scalable Knowledge Management:** Provide mechanisms for creating, updating, and searching large corpora efficiently.

## 3. Components

### 3.1. RAG Corpus Creation (Vertex AI)

*   **Purpose:** To ingest raw data and transform it into an embeddable and searchable format suitable for retrieval.
*   **Mechanism:** Utilizes `rag.RagEmbeddingModelConfig` for configuring embedding models (e.g., `text-embedding-005` from Vertex AI) and `rag.RagVectorDbConfig` for backend vector database configuration.
*   **Function:** `create_RAG_corpus()`
*   **Key Operations:**
    *   Configures an embedding model (e.g., `publishers/google/models/text-embedding-005`).
    *   Sets up a vector database backend configuration.
    *   Calls `rag.create_corpus` to instantiate a new corpus with a specified `display_name`.
    *   Persists the corpus name for later use (e.g., in environment variables).

### 3.2. RAG Retrieval Mechanism

*   **Purpose:** To efficiently query the RAG corpus and retrieve the most relevant contextual information based on a user's query.
*   **Mechanism:** Employs `rag.retrieval_query` to search across specified RAG resources and corpora.
*   **Function:** `search_memory()`
*   **Key Operations:**
    *   Accepts `app_name`, `user_id`, and a `query`.
    *   Performs retrieval using configured `rag_resources`, `rag_corpora`, `similarity_top_k`, and `vector_distance_threshold`.
    *   Filters retrieved contexts based on `source_display_name` to ensure relevance to specific `app_name.user_id` combinations.
    *   Parses retrieved text content (assumed to be JSON lines representing events) into `Event` objects.
    *   Merges and sorts events to form coherent `MemoryEntry` objects.
*   **Outputs:** `SearchMemoryResponse` containing relevant `MemoryEntry` objects.

### 3.3. GraphRAG Indexing

*   **Purpose:** Provides an advanced indexing mechanism that leverages graph structures to represent and retrieve knowledge, potentially offering more nuanced and connected context than traditional vector-only retrieval.
*   **Mechanism:** Configures and executes a GraphRAG indexing pipeline.
*   **Function:** `graphrag_indexing()`
*   **Key Operations:**
    *   Takes `note_list`, `graph_input_dir`, `output_dir`, `lang` as inputs.
    *   Reads and updates GraphRAG configuration from `settings.yaml` and `.env` files.
    *   Integrates with `UserLLMConfigService` to dynamically fetch chat and embedding model configurations (API keys, endpoints, model names).
    *   Modifies prompt templates (`extract_graph.txt`, `summarize_descriptions.txt`) to incorporate language specifics.
    *   Executes a shell script (`graphrag_indexing.sh`) to run the GraphRAG process.
    *   Includes a `_clean_graphrag_keys()` function for managing GraphRAG configurations.
    *   Performs post-processing to create mappings (e.g., `creat_mapping`).
*   **Dependencies:** External GraphRAG tools and configurations (YAML files, shell scripts).

### 3.4. Integration with Agents

*   **Purpose:** To expose RAG capabilities to LLM-powered agents as callable tools, allowing agents to perform information retrieval on demand.
*   **Mechanism:** Agents are configured with `VertexAiRagRetrieval` as a tool.
*   **Example:** `test_vertex_rag_retrieval_for_gemini_1_x()`
*   **Key Aspects:**
    *   An `Agent` is initialized with a `model` (e.g., `gemini-1.5-pro`) and a list of `tools`.
    *   `VertexAiRagRetrieval` tool is provided with `name`, `description`, and a list of `rag_corpora` (e.g., specific Vertex AI corpus paths).
    *   Agents can then invoke this tool to retrieve information based on their internal reasoning.

## 4. Inputs

*   **Raw Data:** Documents, text, or structured information to be indexed.
*   **LLM Configurations:** API keys, model names, endpoints for chat and embedding models (for GraphRAG).
*   **RAG Corpus Identifiers:** Paths or names of existing RAG corpora in Vertex AI.
*   **User Queries:** Natural language questions or requests from users/agents.
*   **Application/User IDs:** For contextualizing retrieval (`app_name`, `user_id`).
*   **GraphRAG Specific Inputs:** `note_list`, `graph_input_dir`, `output_dir`, `lang` for GraphRAG indexing.

## 5. Outputs

*   **Retrieved Contexts:** Text segments or structured data relevant to a query.
*   **Memory Entries:** Formatted and merged historical interaction data.
*   **GraphRAG Artifacts:** Indexed graph data, entity/relation extractions, reports.
*   **Enhanced LLM Responses:** More accurate and informed generated text from the LLM.

## 6. Workflow

1.  **Corpus Preparation:** Raw data is processed, embedded, and indexed into a RAG corpus (either a standard vector store or a GraphRAG index).
2.  **Query Formulation:** A user or agent submits a query.
3.  **Retrieval:** The RAG retrieval mechanism (`search_memory` or agent tool invocation) identifies and fetches relevant contexts from the prepared corpus.
4.  **Augmentation:** The retrieved contexts are combined with the original query and provided to the Gemini LLM.
5.  **Generation:** The Gemini LLM generates a response, leveraging the augmented context.

## 7. Constraints & Considerations

*   **Vertex AI Dependency:** The current RAG implementation heavily relies on Vertex AI for embedding models and corpus management.
*   **API Key Management:** Secure handling of API keys for LLM providers (especially for GraphRAG) is crucial.
*   **Latency:** Retrieval latency needs to be managed to ensure real-time user experiences.
*   **Cost:** Usage of embedding models and LLMs for RAG incurs costs.
*   **Data Freshness:** Strategies for keeping the RAG corpus up-to-date are necessary.
*   **GraphRAG Complexity:** GraphRAG introduces additional complexity in setup, configuration, and maintenance compared to simpler vector stores.
*   **Schema Sanitization:** `_sanitize_schema_formats_for_gemini` suggests careful handling of schema for tool definitions, which might apply to RAG tool integration.

## 8. Testing Strategy

*   **Unit Tests:** Verify individual RAG components (e.g., corpus creation, embedding, parsing events).
*   **Integration Tests:**
    *   `test_rag_retrieval_success()`: Validate successful retrieval scenarios for agents.
    *   `test_rag_retrieval_fail()`: Verify correct error handling for invalid or non-existent retrievals.
    *   Test the end-to-end flow from query to augmented LLM response.
*   **Performance Tests:** Evaluate retrieval latency and throughput.
*   **Accuracy Tests:** Measure the relevance and quality of retrieved contexts and the improvement in LLM response accuracy.
