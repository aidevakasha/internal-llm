# internal-llm

`internal-llm` is a Open WebUI-based local AI workspace. The stack in this repository provisions:

- Open WebUI as the main chat and RAG interface
- Qdrant as the vector database
- Apache Tika for document parsing
- A bridge to an Ollama instance running on the host machine

## Architecture

![Internal LLM architecture diagram](assets/Internal%20LLM.drawio.png)

The diagram shows the broader intended topology around Open WebUI, including hosted and self-hosted model options. The Docker Compose setup in this repo focuses on the Open WebUI stack plus Qdrant and Tika, with Ollama reachable from the host.

## Services

| Service | Local URL | Purpose |
| --- | --- | --- |
| Open WebUI | <http://localhost:3000> | Chat UI, RAG, agents, and document workflows |
| Qdrant | <http://localhost:6333> | Vector storage for embeddings and retrieval |
| Apache Tika | <http://localhost:9998> | Document extraction and text parsing |
| Ollama on host | <http://host.docker.internal:11434> | Local model server used by Open WebUI |

## Prerequisites

- Docker and Docker Compose
- An Ollama instance running on your host machine

## Setup

1. Start Ollama on your host and make sure it listens on port `11434`.
2. Start the stack:

```bash
docker compose up -d
```

1. Open Open WebUI at <http://localhost:3000>.

## Configuration

The main settings live in [`docker-compose.yaml`](docker-compose.yaml):

- `WEBUI_NAME=AI Workspace`
- `ENABLE_OLLAMA_API=True`
- `OLLAMA_BASE_URLS=http://host.docker.internal:11434`
- `VECTOR_DB=qdrant`
- `QDRANT_HOST=qdrant`
- `QDRANT_PORT=6333`
- `QDRANT_URI=http://qdrant:6333`
- `TIKA_SERVER_URL=http://tika:9998`

If you want to connect Open WebUI to a different Ollama endpoint, update `OLLAMA_BASE_URLS` in the compose file.

## Notes

- `open-webui-data` and `qdrant-data` are persisted as Docker volumes.
- `host.docker.internal` is mapped to the host gateway so the container can reach services running on the host.
- The repository currently contains the architecture diagram in `assets/` and the Docker Compose stack definition.
