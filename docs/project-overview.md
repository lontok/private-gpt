# PrivateGPT Project Overview

This document provides three levels of detail about PrivateGPT: Low (Executive Summary), Medium (Technical Overview), and High (Deep Dive).

---

## 📄 Low Fidelity: Executive Summary

**PrivateGPT** is a production-ready AI application that enables organizations to interact with their documents using Large Language Models (LLMs) while maintaining 100% data privacy. All processing happens locally - no data ever leaves your environment.

**Target Audience:** Enterprises, healthcare providers, legal firms, and any organization with sensitive data requiring AI capabilities without compromising privacy.

**Value Proposition:** Get ChatGPT-like capabilities for your documents without sending data to external services. Deploy on-premise or in your private cloud with complete control.

**Technology:** Built with Python, FastAPI, and LlamaIndex. Runs local AI models (like Llama 3) using Ollama, with document embeddings stored in Qdrant vector database.

**Quick Start:**
1. Install with Poetry: `poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"`
2. Pull a model: `ollama pull llama3`
3. Run: `PGPT_PROFILES=ollama make run` and access http://localhost:8001

---

## 🔧 Medium Fidelity: Technical Overview

### Architecture Overview

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Gradio UI     │────▶│   FastAPI       │────▶│   LlamaIndex    │
│   (Browser)     │     │   Server        │     │   RAG Pipeline  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                │                         │
                                ▼                         ▼
                        ┌─────────────────┐     ┌─────────────────┐
                        │   Components    │     │  Vector Store   │
                        │  (LLM, Embed)   │     │   (Qdrant)      │
                        └─────────────────┘     └─────────────────┘
```

### Core Components

1. **API Layer** - FastAPI server following OpenAI standards
2. **RAG Pipeline** - LlamaIndex-based retrieval augmented generation
3. **LLM Component** - Supports multiple providers (Ollama, OpenAI, Azure, etc.)
4. **Vector Store** - Document embeddings storage (Qdrant, Chroma, Postgres)
5. **UI Layer** - Gradio-based web interface

### Technology Stack

#### Default Stack (Out of the Box)
- **Language:** Python 3.11
- **API Framework:** FastAPI
- **UI Framework:** Gradio
- **RAG Engine:** LlamaIndex
- **LLM Provider:** Ollama (with Llama 3)
- **Embedding Model:** Nomic Embed Text (via Ollama)
- **Vector Database:** Qdrant
- **Document Store:** Simple JSON storage

#### Supported Alternatives

| Component | Default | Alternatives |
|-----------|---------|-------------|
| **LLM Provider** | Ollama | LlamaCPP, OpenAI, Azure OpenAI, Google Gemini, AWS Sagemaker |
| **Embedding Provider** | Ollama | HuggingFace, OpenAI, Azure OpenAI, Google Gemini, MistralAI |
| **Vector Database** | Qdrant | Chroma, Milvus, PostgreSQL + pgvector, ClickHouse |
| **Document Store** | Simple (JSON) | PostgreSQL |
| **Deployment** | Local/Bare Metal | Docker, Kubernetes, Cloud VMs |

### Key Features

- 🔒 **100% Private** - Run entirely offline
- 📄 **Document Ingestion** - PDF, Word, TXT, and more
- 💬 **Conversational Interface** - Context-aware chat
- 🔌 **Extensible** - Plugin architecture for components
- 📊 **Production Ready** - REST API, monitoring, scaling
- 🗑️ **Privacy by Design** - Original files not stored after processing

### Installation & Setup

```bash
# Prerequisites
- Python 3.11 (not 3.12+)
- Poetry 1.8.3+
- Ollama (for local models)

# Install
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"

# Configure (settings-ollama.yaml)
ollama:
  llm_model: llama3
  embedding_model: nomic-embed-text

# Run
PGPT_PROFILES=ollama make run
```

### Common Use Cases

1. **Document Q&A** - Query internal documentation
2. **Knowledge Base** - Build searchable repositories
3. **Compliance** - Analyze documents without data exposure
4. **Research** - Literature review with citations
5. **Customer Support** - Private helpdesk automation

---

## 🔬 High Fidelity: Deep Dive

### Detailed Architecture

#### Service Layer Structure

```
private_gpt/
├── server/          # API Layer
│   ├── chat/        # Conversational endpoints
│   ├── completions/ # Text generation
│   ├── embeddings/  # Vector generation
│   ├── chunks/      # Document retrieval
│   └── ingest/      # Document processing
├── components/      # Core implementations
│   ├── llm/         # LLM providers
│   ├── embedding/   # Embedding models
│   ├── vector_store/# Storage backends
│   └── ingest/      # Document parsers
└── ui/              # Gradio interface
```

#### Component Interactions

1. **Request Flow:**
   ```
   UI Request → FastAPI Router → Service Layer → Component Layer → LlamaIndex → Response
   ```

2. **Document Ingestion:**
   ```
   File Upload → Temporary File → Parser → Chunking → Embedding → Vector Store → Index Update → Delete Original
   ```

3. **RAG Pipeline:**
   ```
   Query → Embedding → Vector Search → Context Retrieval → LLM Prompt → Response
   ```

### Configuration System

#### Profile Management
```yaml
# settings-{profile}.yaml structure
server:
  env_name: ${APP_ENV:production}
  port: ${PORT:8001}

data:
  local_data_folder: local_data/private_gpt

llm:
  mode: ollama
  max_new_tokens: 512
  context_window: 3900
  temperature: 0.1

embedding:
  mode: ollama
  
vectorstore:
  database: qdrant

qdrant:
  path: local_data/private_gpt/qdrant
```

#### Environment Variables
```bash
PGPT_PROFILES=ollama,custom  # Comma-separated profiles
PGPT_PORT=8001              # Server port
OLLAMA_HOST=localhost:11434 # Ollama endpoint
```

### API Endpoints

#### Core Endpoints
- `POST /v1/chat/completions` - Chat with context
- `POST /v1/completions` - Direct completion
- `POST /v1/embeddings` - Generate embeddings
- `GET /v1/chunks` - Retrieve document chunks
- `POST /v1/ingest/file` - Upload document
- `GET /api/health` - Health check

#### Request/Response Format
```json
// Chat Request
{
  "messages": [{"role": "user", "content": "What is..."}],
  "use_context": true,
  "context_filter": {"docs_ids": ["doc1", "doc2"]},
  "stream": false
}

// Streaming Response
data: {"choices": [{"delta": {"content": "The answer"}}]}
```

### Extension Points

1. **Custom LLM Provider:**
   ```python
   class CustomLLM(LLM):
       def complete(self, prompt: str) -> str:
           # Implementation
   ```

2. **Custom Vector Store:**
   ```python
   class CustomVectorStore(VectorStore):
       def query(self, embedding: List[float]) -> List[Document]:
           # Implementation
   ```

3. **Custom Document Parser:**
   ```python
   @injectable
   class CustomParser(BaseParser):
       def parse(self, file: Path) -> List[Document]:
           # Implementation
   ```

### Logging and Monitoring

#### Log Configuration
```python
# Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
# Set via environment: PGPT_LOG_LEVEL=DEBUG

# Default log locations:
# - Console output (stdout)
# - File: privategpt.log (when using nohup)
# - Structured logs with timestamps
```

#### Monitoring User Interactions

1. **Enable Debug Logging:**
   ```bash
   PGPT_LOG_LEVEL=DEBUG PGPT_PROFILES=ollama make run
   ```

2. **Key Log Patterns:**
   ```
   # User uploads document
   INFO: POST /v1/ingest/file - Document: report.pdf
   
   # User asks question
   INFO: POST /v1/chat/completions - Query: "What is the summary?"
   DEBUG: Retrieved 5 chunks from vector store
   DEBUG: Context length: 2048 tokens
   
   # Response generation
   INFO: Generated response in 3.2s
   DEBUG: Token usage: prompt=512, completion=128
   ```

3. **Real-time Monitoring:**
   ```bash
   # Watch logs in real-time
   tail -f privategpt.log | grep -E "(POST|GET|chunks|Query)"
   
   # Monitor performance
   tail -f privategpt.log | grep -E "(Generated response|Token usage)"
   ```

4. **Custom Logging:**
   ```python
   # Add to any component
   import logging
   logger = logging.getLogger(__name__)
   
   logger.info(f"Processing document: {filename}")
   logger.debug(f"Chunk count: {len(chunks)}")
   ```

5. **Log Analysis Tools:**
   ```bash
   # Count requests by type
   grep "POST" privategpt.log | awk '{print $7}' | sort | uniq -c
   
   # Average response time
   grep "Generated response" privategpt.log | awk '{sum+=$5; count++} END {print sum/count}'
   ```

### Performance Considerations

1. **Model Selection:**
   - Llama 3 8B: Best quality/performance balance
   - Mistral 7B: Faster, good for simple queries
   - Llama 2 7B: Legacy option, widely compatible

2. **Chunking Strategy:**
   - Default: 512 tokens with 50 token overlap
   - Adjust based on document types
   - Smaller chunks = better precision, more requests

3. **Vector Store Optimization:**
   - Qdrant: Best for <1M documents
   - Postgres: Better for larger scale
   - Index regularly for performance

4. **Caching:**
   - LLM responses cached by default
   - Embedding cache reduces redundant computation
   - Clear cache: `make wipe`

### Data Storage

#### File Storage Architecture

1. **Raw Files (NOT Stored):**
   - Uploaded files processed in memory/temp files
   - Immediately deleted after text extraction
   - Privacy by design - no originals kept

2. **Vector Data:**
   - **Location:** `local_data/private_gpt/qdrant/`
   - **Storage:** SQLite database with embeddings
   - **Purpose:** Semantic search capabilities

3. **Document Data:**
   - **Location:** `local_data/private_gpt/`
   - **Files:**
     - `docstore.json` - Text chunks and metadata
     - `index_store.json` - Index information
     - `graph_store.json` - Document relationships
   - **Content:** Processed text, filenames, page numbers

4. **Storage Management:**
   ```bash
   # Clear all data
   make wipe
   
   # Backup data
   cp -r local_data/private_gpt /backup/location
   
   # Check storage size
   du -sh local_data/private_gpt
   ```

### Security Model

1. **Data Privacy:**
   - No external API calls (in local mode)
   - All data stored locally
   - No telemetry or tracking
   - Original documents never persisted

2. **Access Control:**
   - Basic auth via middleware
   - API key authentication
   - Document-level permissions (enterprise)

3. **Network Security:**
   - HTTPS support via reverse proxy
   - Firewall port 8001
   - VPN for remote access

### Troubleshooting Guide

#### Common Issues

1. **"Local dependencies not found"**
   ```bash
   # Solution: Install correct extras
   poetry install --extras "llms-ollama embeddings-ollama"
   ```

2. **"Connection refused on port 11434"**
   ```bash
   # Solution: Start Ollama
   ollama serve
   ```

3. **"Model not found"**
   ```bash
   # Solution: Pull the model
   ollama pull llama3
   ```

4. **Slow responses**
   - Check: Model size vs available RAM
   - Try: Smaller model or increase context window
   - Monitor: `ollama ps` for active models

### Development Workflow

1. **Local Development:**
   ```bash
   make dev  # Auto-reload enabled
   ```

2. **Testing:**
   ```bash
   make test           # Run tests
   make test-coverage  # With coverage
   ```

3. **Code Quality:**
   ```bash
   make check  # Format + type check
   ```

4. **Adding Features:**
   - Create service in `server/{feature}/`
   - Implement component in `components/{feature}/`
   - Add tests in `tests/{feature}/`
   - Update OpenAPI spec: `make api-docs`

### Production Deployment

1. **Docker:**
   ```yaml
   # docker-compose.yml included
   docker-compose up -d
   ```

2. **Kubernetes:**
   - Helm charts available
   - Horizontal scaling supported
   - Persistent volume for vector store

3. **Monitoring:**
   - Prometheus metrics endpoint
   - Health checks for load balancers
   - Log aggregation ready

---

## 📚 Additional Resources

- **Documentation:** https://docs.privategpt.dev/
- **GitHub:** https://github.com/zylon-ai/private-gpt
- **Discord:** Community support
- **Enterprise:** https://zylon.ai for managed deployment