# Local RAG API with FastAPI, Ollama & ChromaDB

A professional, local-first Retrieval-Augmented Generation (RAG) API that allows you to index personal knowledge base documents and query them securely on your own machine. Running entirely on-device, this system guarantees zero cloud API costs and complete data privacy.

---

## ⚡️ Architecture Overview
This API automates the three distinct components of a modern RAG pipeline:
1. **Retrieval**: When a query is received, the input is converted into a vector embedding using the local `nomic-embed-text` model. ChromaDB performs a high-dimensional vector search to pull the top 2 most semantically relevant text chunks.
2. **Augmentation**: The system dynamically inserts the retrieved context chunks directly into a prompt template alongside the user's original query.
3. **Generation**: The compiled, context-grounded prompt is sent to `qwen2.5:0.5b` running locally via Ollama to generate an accurate, hallucination-free response.

---

## 🛠️ Built With
- **FastAPI**: Modern, high-performance web framework for building APIs with Python.
- **ChromaDB**: Lightweight, open-source vector database built for semantic AI workflows.
- **Ollama**: Local containerization wrapper used to host and serve lightweight LLMs on standard laptops.
- **nomic-embed-text**: 8192-context length text embedding model.
- **qwen2.5:0.5b**: Highly efficient, low-latency 0.5 billion parameter language model.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.13 (highly recommended due to ChromaDB compatibility requirements on newer Python releases).
- Ollama installed locally.

Ensure your Ollama daemon is running and pull the necessary models:
```bash
# Pull the embedding model
ollama pull nomic-embed-text

# Pull the lightweight LLM
ollama pull qwen2.5:0.5b
```

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/VishnuVardhan3006/Rag-api.git
   cd nextwork-rag-api
   ```

2. **Initialize a Virtual Environment:**
   For Windows PowerShell (forcing Python 3.13 execution via the Python Launcher):
   ```powershell
   py -3.13 -m venv venv
   venv\Scripts\activate
   ```
   For macOS / Linux:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies:**
   ```bash
   pip install fastapi uvicorn chromadb ollama
   ```

4. **Populate Your Vector Database:**
   Ensure your source context file (e.g., `profile.txt`) is in your directory, then run the script to generate embeddings and save them to your database cache:
   ```bash
   python build_knowledge_base.py
   ```

5. **Run the API Server:**
   ```bash
   uvicorn main:app --reload
   ```

---

## 🔌 API Documentation

### 💻 Swagger UI
Once your server is running, FastAPI automatically compiles interactive documentation. You can test your endpoints directly at:
👉 [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

### 📌 GET `/ask`
Queries your local personal database and generates an AI-grounded response.

**Query Parameters:**
- `question` (string, required): The query you want to ask the LLM.

**Example Request:**
```bash
curl "http://127.0.0.1:8000/ask?question=What%20are%20my%20career%20goals%3F"
```

**Example Response Payload:**
```json
{
  "question": "What are my career goals?",
  "answer": "Based on your profile, your primary career goal is to master backend API development and specialize in local, containerized DevSecOps pipelines.",
  "context_used": [
    "I am currently studying DevSecOps. My professional focus is building reliable local API infrastructures.",
    "My immediate learning goal is mastering containerization using Docker for seamless app distribution."
  ]
}
```

---

## 🔒 Security & Privacy Features
- **100% Local Execution**: No data ever leaves your computer. Prompts are evaluated locally.
- **Zero Cost**: Completely free to run with no commercial API tokens or monthly limits.
- **Clean Git Hygiene**: Includes a robust `.gitignore` ensuring that database binaries (`.sqlite3`, `.bin`) and virtual environment packages (`venv/`) remain isolated to local environments.
