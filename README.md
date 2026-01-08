# 🚀 Spring-AI-Ollama-LLM

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17%2B-orange)](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)](https://spring.io/projects/spring-boot)

Elegant Spring Boot starter / example for building LLM-powered APIs using Ollama (local or remote LLMs). This project demonstrates how to connect Spring services to an Ollama model to power chat, completion, and assistant-like endpoints.

- Clean, modular Spring Boot architecture
- Easy configuration to point at a local Ollama instance or a remote LLM endpoint
- Example controllers, services, and DTOs for LLM requests and responses
- Docker-friendly: run locally or in containers

---

## 🔍 What is this for?

If you want a lightweight Spring-based backend that delegates natural-language tasks to an Ollama model (or any HTTP-compatible LLM server), this project gives you:
- A repeatable pattern for wiring LLM calls into your service layer
- Example request/response DTOs and error handling
- Tips for local development with Ollama

---

## ✨ Features

- Spring Boot REST API to interact with an LLM model
- Pluggable configuration (change model / endpoint via properties or env vars)
- Example endpoints for chat/completion flows
- Health and readiness checks
- Example Dockerfile and guidance for containerized runs

---

## 🧩 Tech Stack

- Spring Boot
- Java 17+
- Spring Web, Spring Boot Actuator
- (Optional) Lombok, Spring Validation
- Ollama (local model/runtime) or any HTTP LLM endpoint
- Docker / Docker Compose for containerized runs

---

## ⚙️ Prerequisites

- Java 17 or later
- Maven (or use the bundled `mvnw`) or Gradle
- (Recommended) [Ollama](https://ollama.com) installed and a model available locally, or access to any LLM HTTP endpoint
- Docker (optional, for containers)

---

## 🚀 Quick Start

1. Clone the repo
```bash
git clone https://github.com/Amitkarane/Spring-Ai-Ollama-LLM.git
cd Spring-Ai-Ollama-LLM
```

2. Configure your LLM connection

You can configure connection in `src/main/resources/application.yml` or via environment variables. Example environment variables:

- OLLAMA_URL — base URL for the Ollama/LLM HTTP API (e.g. `http://localhost:11434`)
- OLLAMA_MODEL — model name to use (e.g. `llama2`, `gpt-4o-mini`, or your local model id)
- OLLAMA_API_KEY — if your LLM endpoint requires an API key

Example application properties (application.yml snippet):
```yaml
llm:
  url: ${OLLAMA_URL:http://localhost:11434}
  model: ${OLLAMA_MODEL:my-local-model}
  apiKey: ${OLLAMA_API_KEY:}
  timeoutMs: 60000
```

3. Run the app
```bash
# with Maven wrapper
./mvnw spring-boot:run

# or build and run jar
./mvnw clean package
java -jar target/spring-ai-ollama-llm-0.0.1-SNAPSHOT.jar
```

The API will start on `http://localhost:8080` by default.

---

## 📦 Example Endpoints

> NOTE: the exact endpoint paths used in your project may differ. These examples follow the common pattern implemented here.

- POST /api/v1/llm/chat  
  Send a chat-style request and get the model response.

Request (JSON):
```json
{
  "messages": [
    { "role": "user", "content": "Write a short haiku about coffee." }
  ],
  "temperature": 0.7,
  "maxTokens": 200
}
```

Response (JSON):
```json
{
  "id": "response-123",
  "model": "my-local-model",
  "choices": [
    {
      "role": "assistant",
      "content": "Morning steam rising\nDark liquid warms sleepy hands\nDay begins anew"
    }
  ]
}
```

- GET /actuator/health  
  Basic Spring Boot health endpoint.

---

## 🛠️ Configuration Tips

- For local development with Ollama, install and run Ollama and load a model:
  - See the official site: [Ollama](https://ollama.com)
  - Run a model or ensure your Ollama daemon is available and the HTTP API host/port are reachable.

- Use environment variables in CI/CD to keep keys and model names out of source control:
  - OLLAMA_URL, OLLAMA_MODEL, OLLAMA_API_KEY

- Increase request timeouts for large model responses:
  - Adjust `llm.timeoutMs` in properties.

---

## 🧪 Example: Calling from curl

```bash
curl -X POST "http://localhost:8080/api/v1/llm/chat" \
  -H "Content-Type: application/json" \
  -d '{
    "messages":[{"role":"user","content":"Summarize the benefits of unit testing."}],
    "temperature":0.2
  }'
```

---

## 🐳 Docker (optional)

Build the app image:
```bash
docker build -t spring-ai-ollama-llm:latest .
```

Run (make sure Ollama or the target LLM endpoint is reachable from the container):
```bash
docker run --rm -e OLLAMA_URL=http://host.docker.internal:11434 -e OLLAMA_MODEL=my-model -p 8080:8080 spring-ai-ollama-llm:latest
```

(If running Docker on Linux, replace `host.docker.internal` with the appropriate host IP.)

---

## 📐 Architecture Overview

- Controller layer: HTTP endpoints that accept prompts and chat messages
- Service layer: Business logic and orchestration of LLM calls
- Client layer: A small HTTP client responsible for calling Ollama (or any LLM endpoint), handling retries, timeouts, and response mapping
- DTOs: Clean request/response models to decouple API from transport format

---

## ✅ Best Practices / Recommendations

- Sanitize and validate user inputs before sending them to the model.
- Apply rate limits to avoid unexpected costs or overloading local resources.
- Log prompts and responses carefully — avoid storing sensitive user data.
- Use streaming (if supported) for long responses to improve UX.
- Add caching for repeated deterministic prompts where appropriate.

---

## 🤝 Contributing

Contributions are welcome! Typical ways to help:
- Open issues for bugs or feature requests
- Submit PRs for bug fixes, docs, or improvements
- Add example integrations or additional LLM client adapters

Please follow standard GitHub conventions and include tests where applicable.

---

## 🧾 License

This project is released under the MIT License. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- [Ollama](https://ollama.com) — for local LLM runtime inspiration
- Spring and the awesome open-source ecosystem

---

## ✉️ Contact

Created by Amit Karane — happy to help if you open an issue or PR in this repository.

Enjoy building with Spring + Ollama! 🌟
