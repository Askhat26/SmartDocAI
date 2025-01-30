# FastAPI with Caching and Document Management

This repository contains a FastAPI-based backend application for managing documents and answering user queries using a Retrieval-Augmented Generation (RAG) model. It supports features like document upload, deletion, chat-based querying, and caching for performance optimization.

---

## Features

### 1. **Document Management**
- Upload and index documents (.pdf, .docx, .html).
- List all indexed documents.
- Delete specific documents from the system and underlying vector store.

### 2. **Chat-Based Querying**
- Ask questions using a Retrieval-Augmented Generation (RAG) model.
- Automatically reformulates user queries to contextualize with chat history.

### 3. **Caching Mechanisms**
- Redis caching for frequent queries and static data (e.g., document list).
- Reduced response times for repeated API calls.

### 4. **Logging**
- Logs all API activity for easier debugging and auditing.

---

## Tech Stack

- **Backend Framework**: FastAPI
- **Vector Store**: Chroma
- **LLM Integration**: LangChain + OpenAI
- **Caching**: Redis with `fastapi-cache2`
- **Database**: Placeholder for document metadata management
- **File Handling**: Temporary file storage during uploads

---

## Prerequisites

### 1. Install Dependencies
Ensure you have Python 3.8+ installed. Install the required libraries using pip:

```bash
pip install -r requirements.txt
```

### 2. Set Up Redis
Run Redis locally using Docker:

```bash
docker run -d -p 6379:6379 redis
```

### 3. Environment Variables
Create a `.env` file to store environment variables like API keys and Redis connection details. Example:

```
OPENAI_API_KEY=your_openai_api_key
REDIS_URL=redis://localhost:6379
```

---

## Getting Started

### 1. Run the Server
Start the FastAPI server:

```bash
uvicorn main:app --reload
```

The application will be available at `http://127.0.0.1:8000`.

### 2. API Endpoints

#### **Upload Document**
- **Endpoint**: `POST /upload-doc`
- **Description**: Upload a document and index it in the vector store.
- **Payload**: Multipart file upload (.pdf, .docx, .html).

#### **List Documents**
- **Endpoint**: `GET /list-docs`
- **Description**: Retrieve a list of all indexed documents.

#### **Delete Document**
- **Endpoint**: `POST /delete-doc`
- **Description**: Delete a document from the vector store and database.
- **Payload**:
  ```json
  {
    "file_id": "document_id"
  }
  ```

#### **Chat**
- **Endpoint**: `POST /chat`
- **Description**: Ask a question, and the system will use RAG to generate an answer.
- **Payload**:
  ```json
  {
    "question": "your_question",
    "model": "gpt-4",
    "chat_history": [
      {"role": "user", "content": "..."},
      {"role": "assistant", "content": "..."}
    ]
  }
  ```

---

## Caching Implementation

### Redis Integration
- Redis is used to cache frequent queries and static data like the document list.
- Cache expiration is set per endpoint (e.g., 1 hour for `list-docs`, 10 minutes for chat responses).

### Cache Decorators
- `@cache(expire=3600)` applied to `/list-docs`.
- `@cache(expire=600)` applied to `/chat` responses.

### Clearing Cache
Use `FastAPICache.clear()` to clear all cached entries or specific namespaces when data changes.

---

## File Structure

```
.
├── main.py                  # Main FastAPI application
├── langchain_utils.py       # Utilities for LangChain integration
├── db_utils.py              # Database helper functions
├── chroma_utils.py          # Chroma vector store integration
├── pydantic_models.py       # Request and response models
├── requirements.txt         # Python dependencies
├── app.log                  # Application log file



## Contributing
Feel free to fork this repository and submit pull requests for any enhancements or bug fixes!

