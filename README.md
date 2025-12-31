🧠 RAG-Based Document Q&A System (FastAPI + LangChain + Chroma)
📌 Overview

This project is a Retrieval-Augmented Generation (RAG) backend API built using FastAPI, LangChain, OpenAI models, and ChromaDB.

It allows users to:

Upload documents (PDF, DOCX, HTML)

Index them into a vector database (Chroma)

Ask conversational questions over the uploaded documents

Maintain session-based chat history

Delete indexed documents safely

Switch between OpenAI models dynamically

The system is designed for document intelligence, enterprise search, and AI assistants.

🎯 Why This Project Exists

Large Language Models alone:

Hallucinate

Forget long documents

Lack traceability

This project solves that by:

Storing documents in a vector database

Retrieving only relevant chunks

Combining them with chat history

Generating grounded, contextual answers

This architecture is suitable for:

Internal knowledge bases

Policy & compliance assistants

Research tools

Customer support bots

🏗️ System Architecture
Client (UI / API Consumer)
        |
        v
FastAPI Backend
 ├── /chat (RAG Q&A)
 ├── /upload-doc (Index documents)
 ├── /list-docs (Metadata)
 └── /delete-doc (Cleanup)
        |
        v
LangChain
 ├── History-aware retriever
 ├── Prompt templates
 └── Retrieval chain
        |
        v
ChromaDB (Vector Store)
        |
        v
OpenAI (Chat + Embeddings)

🚀 Features
✅ Conversational RAG Chat

Session-based memory

Contextual question reformulation

History-aware retrieval

✅ Document Management

Upload & index documents

Supported formats:

PDF

DOCX

HTML

Delete documents cleanly from:

ChromaDB

SQLite metadata store

✅ Model Flexibility

Supported models:

gpt-4o

gpt-4o-mini

gpt-3.5 (default)

✅ Persistent Chat History

Stored in SQLite

Used for follow-up questions

Enables multi-turn conversations

✅ Clean API Contracts

Pydantic request/response models

Typed enums for model selection

🧩 Project Structure
.
├── main.py                # FastAPI app & routes
├── pydantic_models.py     # Request/response schemas
├── langchain_utils.py     # RAG chain logic
├── chroma_utils.py        # Vector DB operations
├── db_utils.py            # SQLite persistence
├── chroma_db/             # Chroma vector store
├── rag_app.db             # SQLite database
├── app.log                # Application logs
└── README.md

🗄️ Database Schema
1️⃣ Chat Logs (application_logs)
id INTEGER PRIMARY KEY
session_id TEXT
user_query TEXT
gpt_response TEXT
model TEXT
created_at TIMESTAMP

2️⃣ Document Store (document_store)
id INTEGER PRIMARY KEY
filename TEXT
upload_timestamp TIMESTAMP

⚙️ Environment Variables

Create a .env file in the root directory:

OPENAI_API_KEY=your_openai_api_key


🔐 Never commit .env files to GitHub

📦 Installation
1️⃣ Clone Repository
git clone <your-repo-url>
cd rag-app

2️⃣ Create Virtual Environment
python -m venv venv
source venv/bin/activate    # macOS / Linux
venv\Scripts\activate       # Windows

3️⃣ Install Dependencies
pip install fastapi uvicorn langchain langchain-openai langchain-chroma chromadb pydantic sqlite3 python-dotenv

▶️ Run the Server
uvicorn main:app --reload


Server will be available at:

http://127.0.0.1:8000


Interactive docs:

Swagger UI → /docs

ReDoc → /redoc

🔌 API Endpoints
🧠 POST /chat

Ask questions over indexed documents.

Request

{
  "question": "What is this document about?",
  "session_id": "optional-session-id",
  "model": "gpt-4o-mini"
}


Response

{
  "answer": "The document explains...",
  "session_id": "uuid",
  "model": "gpt-4o-mini"
}

📤 POST /upload-doc

Upload and index a document.

Form-Data

file: PDF / DOCX / HTML

Response

{
  "message": "File indexed successfully",
  "file_id": 3
}

📄 GET /list-docs

List all indexed documents.

Response

[
  {
    "id": 1,
    "filename": "policy.pdf",
    "upload_timestamp": "2025-01-01T12:00:00"
  }
]

🗑️ POST /delete-doc

Delete a document by ID.

Request

{
  "file_id": 1
}

🧠 RAG Logic Explained

User question arrives

Chat history fetched from SQLite

Question is reformulated (history-aware)

Relevant chunks retrieved from Chroma

LLM answers using retrieved context

Response + logs persisted

This prevents hallucination and improves accuracy.

🔐 Logging & Observability

All queries and responses logged to:

SQLite

app.log

Useful for:

Debugging

Analytics

Auditing

🛠️ Tech Stack
Layer	Technology
API	FastAPI
LLM	OpenAI (ChatOpenAI)
RAG	LangChain
Vector DB	Chroma
Embeddings	OpenAI Embeddings
DB	SQLite
Validation	Pydantic
🚀 Future Improvements

Authentication & API keys

User-level document isolation

Streaming responses

Hybrid search (BM25 + vector)

Docker & CI/CD

Async ingestion pipeline
