# Tableau Course Q&A Chatbot (Gemini + LangChain RAG)

A Retrieval-Augmented Generation (RAG) Q&A assistant built with **LangChain**, **ChromaDB**, and **Google Gemini** to answer student questions across Tableau course lectures with source citations and token streaming.

---

### Overview
This project ingests course lecture transcripts, enriches them with section and lecture metadata, cleans transcription errors via an LLM formatting chain, and indexes token-split chunks into a Chroma vector database. When a student submits a question, the pipeline retrieves relevant context using Maximum Marginal Relevance (MMR) and streams a grounded answer with exact lecture citations.

---

### Architecture & Pipeline
1. **Document Ingestion & Metadata Splitting:** Loads the course transcript PDF and splits it by Markdown headers (`#` Section Title, `##` Lecture Title) using `MarkdownHeaderTextSplitter`.
2. **LLM Transcript Cleanup:** Runs a batch LangChain Expression Language (LCEL) chain (`gemini-3.5-flash-lite`) to restore paragraph structure and fix domain-specific transcription errors (e.g., *Tableau*).
3. **Token-Based Chunking:** Splits cleaned lectures into 500-token chunks (with a 50-token overlap) using `TokenTextSplitter` (`cl100k_base`).
4. **Vector Indexing & MMR Retrieval:** Embeds chunks using `gemini-embedding-001` into a persistent **ChromaDB** vector store and retrieves diverse context passages via Maximum Marginal Relevance (`search_type="mmr"`).
5. **Custom Context Formatting & Streaming:** Formats retrieved documents with their section/lecture metadata via `RunnableLambda` and streams real-time token responses using `.stream()`.

---

### Tech Stack
* **Orchestration:** LangChain (`langchain-core`, `langchain-community`, `langchain-google-genai`, `langchain-chroma`, `langchain-text-splitters`)
* **LLM & Embeddings:** Google Gemini (`gemini-3.5-flash-lite`, `gemini-embedding-001`)
* **Vector Database:** ChromaDB
* **Language & Environment:** Python, Jupyter Notebook

---

### Repository Contents
* [`Tableau_rag_chatbot.ipynb`](./Tableau_rag_chatbot.ipynb) — Complete end-to-end RAG implementation with saved streaming outputs and source citations.

---

### Setup & Usage
1. **Clone the repository:**
   ```bash
   git clone [https://github.com/MsMasingita/tableau-rag-chatbot.git](https://github.com/MsMasingita/tableau-rag-chatbot.git)
   cd tableau-rag-chatbot
