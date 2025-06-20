# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PrivateGPT is a production-ready AI project for private document Q&A using Large Language Models (LLMs). It allows users to interact with their documents without data leaving their execution environment, ensuring 100% privacy.

## Key Development Commands

### Setup and Running
```bash
# Install dependencies with desired extras
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"

# Run the application
make run
# or with specific profile
PGPT_PROFILES=ollama make run

# Development mode with auto-reload
make dev  # Unix/Mac - runs on http://localhost:8001
make dev-windows  # Windows
```

### Code Quality
```bash
# Before committing - runs format + mypy
make check

# Individual quality commands
make format      # Auto-format with black and ruff
make black       # Check black formatting
make ruff        # Run ruff linter
make mypy        # Type checking
```

### Testing
```bash
make test         # Run all tests
make test-coverage # Run tests with coverage report

# Run a single test
PYTHONPATH=. poetry run pytest tests/path/to/test_file.py::test_function_name
```

### Utility Commands
```bash
make ingest <folder_path>  # Ingest documents from folder
make setup                 # Download models for local setup
make wipe                  # Clear all ingested data
make api-docs              # Generate OpenAPI documentation
make stats                 # Show statistics
```

## Architecture Overview

### API Structure (`private_gpt/server/`)
The API follows OpenAI standards and is organized by feature:
- **chat/**: Conversational interface with context
- **completions/**: Direct text generation
- **embeddings/**: Generate embeddings for text
- **chunks/**: Retrieve relevant document chunks
- **ingest/**: Document ingestion and processing
- **health/**: Health check endpoint

Each API package contains:
- `*_router.py`: FastAPI route definitions
- `*_service.py`: Business logic implementation using abstractions

### Components Layer (`private_gpt/components/`)
Provides implementations for core abstractions:
- **llm/**: LLM providers (Ollama, OpenAI, LlamaCPP, etc.)
- **embedding/**: Embedding model implementations
- **vector_store/**: Document storage backends (Qdrant, Chroma, etc.)
- **node_store/**: Document metadata storage
- **ingest/**: Document parsing and chunking logic

### Key Design Patterns
1. **Dependency Injection**: Uses `injector` library for decoupling
2. **Abstract Interfaces**: Components implement LlamaIndex abstractions
3. **Profile-based Configuration**: Different settings for different deployments
4. **Service Layer Pattern**: Clean separation between API and business logic

## Configuration System

### Profile Selection
Use `PGPT_PROFILES` environment variable:
- `local`: Local LlamaCPP models
- `ollama`: Ollama backend (default)
- `openai`: OpenAI API
- `azopenai`: Azure OpenAI
- `sagemaker`: AWS Sagemaker
- `gemini`: Google Gemini

### Settings Files
Each profile has a corresponding `settings-{profile}.yaml` file. Key sections:
- `llm`: LLM configuration
- `embedding`: Embedding model settings
- `vectorstore`: Vector database configuration
- `rag`: RAG pipeline settings
- `ui`: UI enable/disable and configuration

## Development Guidelines

### Adding New Features
1. Follow the existing service/router pattern in `private_gpt/server/`
2. Implement abstractions in `private_gpt/components/`
3. Use dependency injection for new components
4. Add corresponding tests in `tests/`
5. Update API documentation if adding new endpoints

### Testing Strategy
- Unit tests for components and services
- Integration tests for API endpoints
- Use fixtures from `tests/fixtures/` for test data
- Mock external dependencies (LLMs, vector stores)

### Code Style
- Python 3.11 required (not 3.12+)
- Black for formatting
- Ruff for linting
- MyPy for type checking
- Google docstring convention
- Always run `make check` before committing

### Common Patterns
1. **Document Ingestion**: See `IngestService` for handling various file types
2. **Streaming Responses**: Use `ChatService` patterns for streaming
3. **Context Retrieval**: Check `ContextService` for RAG implementation
4. **Error Handling**: Follow existing error patterns in services

## Important Notes

1. **Python Version**: Strictly requires Python 3.11 (3.12+ not supported due to dependencies)
2. **Poetry Version**: Use 1.8.3+ (known issues with 1.7.0)
3. **Model Management**: 
   - Ollama models are auto-pulled by default
   - LlamaCPP models need `make setup` to download
4. **Default Ports**: API/UI runs on `http://localhost:8001`
5. **GPU Support**: Requires specific compilation flags for Metal (macOS), CUDA (NVIDIA), or ROCm (AMD)