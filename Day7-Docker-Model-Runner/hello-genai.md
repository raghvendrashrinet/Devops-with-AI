### hello-genai initiative leverages Docker's native Docker Model Runner (DMR). 
This feature allows treating AI models like standard container images—pulling, running, and managing them directly from the terminal without setting up separate third-party environments like Ollama or custom Python configurations.
### What hello-genai and Docker Model Runner Help You Do
Instead of dealing with complex Python version matching, installing heavy CUDA drivers manually, or configuring complex inference frameworks, this approach delivers immediate, tangible benefits:
#### 1. Unified Infrastructure (Models as Containers)
It treats AI models as first-class OCI artifacts on Docker Hub. You use the exact same muscle memory you already have for managing applications to manage models:
Instead of docker pull ubuntu, you run 
> docker model pull ai/smollm2

Instead of docker run, you use
> docker model run
#### 2. Zero-Dependency Local Inference
You do not need Python, Conda, or an active cloud subscription to get a model talking. Docker automatically detects your local hardware accelerator—Metal on Mac or CUDA on Linux—and spins up an optimized native binary (like llama.cpp) to run the model directly on your hardware at bare-metal speeds.

#### 3. Immediate OpenAI-Compatible API
The moment you run a model using the Docker Model Runner, it automatically spins up a local background server that exposes a standardized OpenAI-compatible REST API (typically listening on your host network).

This means any application or multi-agent framework (like CrewAI, LangGraph, or AutoGen) can immediately integrate with it by pointing to this endpoint.
#### The Workflow You Are Setting Up
```
┌─────────────────────┐docker model pull ┌──────────────────────┐
│ Docker Hub Registry │ ──── ──────────> │  Your Host Machine   │
└─────────────────────┘                  │                      │
                                         │  ┌────────────────┐  │
                                         │  │  smollm2 Model │  │
                                         │  └────────────────┘  │
                                         │          ▲           │
┌─────────────────┐   Local API Call     │          │ (Inference│
│ Your Custom App │ ─────────────────────┼──────────┘  Engine)  │
│ (Containerized) │  /v1/chat/completions│                      │
└─────────────────┘                      └──────────────────────┘

````
----

1. Pulling: docker model pull ai/smollm2:135M-Q4_K_M streams the quantized weights from Docker Hub straight into your local Docker cache.

2.Inspecting: docker model list gives you a clean view of your active local model inventory, parameter sizing, and architecture definitions.

3.Running: docker model run ai/smollm2:135M-Q4_K_M loads the engine directly into memory, opens an interactive terminal chat loop, and activates the local web server endpoint.

---
### Official docker/hello-genai repository: https://github.com/docker/hello-genai
#### 📂 What is Inside the Repo?
Once cloned, you will notice the repository has a structure that includes multiple mini-applications, each written in a different programming language:
- go-genai/: A lightweight chatbot backend written in Go.
- py-genai/: A chatbot backend written in Python.
- node-genai/: A chatbot backend written in Node.js.
- go-rust/: A chatbot backend written in Rust.
- docker-compose.yml: The orchestrator that glues all these language-specific web servers together and binds them to the model engine.
- run.sh: A helper shell script provided by Docker to automate the setup process.

#### Step 1: Clone the Repo
```
git clone https://github.com/docker/hello-genai.git
cd hello-genai
```
#### Step 2: Select and Pull Your Local Model
Before booting the apps, make sure you have pulled a GGUF format model that the Docker Model Runner can read (like smollm2 or llama3.2):
```
 docker model pull ai/smollm2:135M-Q4_K_M --> dont work 
```
#### Note: models.yaml has the model which compose will download and run 
```
models:
  llama:
    model: ai/llama3.2:1B-Q8_0
    context_size: 2048
```

#### Step 3: Launch Everything via Docker Compose
Instead of managing each language manually, simply run Docker Compose. It compiles all the backend folders and points them automatically to the internal Docker model service:
```
 docker compose up -d
```
##### Note : Alternatively, you can just execute ./run.sh if you are on a Mac/Linux terminal, which automates the build and run stages for you

#### Requirement 
macOS (recent version)
Either:
Docker and Docker Compose (preferred)
Go 1.21 or later
Local LLM server

#### env file  : If you're using a different LLM server configuration, you may need to modify the.env file
#### 🌐 What Does It Help You Do? (The Result)
Once Docker Compose finishes spinning up the containers, it hosts four identical chatbot web interfaces running on different ports simultaneously. You can open your browser and visit whichever environment matches your language preference:

- 🐹 Go Version: http://localhost:8080
- 🐍 Python Version: http://localhost:8081
- 🟢 Node.js Version: http://localhost:8082
- 🦀 Rust Version: http://localhost:8083
  
Why this is awesome for your DevOps journey:  

It proves that the LLM is language-agnostic. Whether your production microservices are built in Python, Go, or Node, they all communicate with the exact same local Docker Model Runner endpoint using standardized environment variables (LLM_BASE_URL and LLM_MODEL_NAME). It is a perfect template for scaling multi-container AI architectures!

#### To make use of existing model which is already installed in the system
1. list the existing models
```
> ollama ls
   NAME               ID              SIZE      MODIFIED
llama3.1:latest    46e0c10c039e    4.9 GB    17 hours ago
llama3.2:1b        baf6a787fdff    1.3 GB    3 days ago
```
2. Start Ollama server (if not already running):
```
ollama serve

This exposes the API at http://localhost:11434
```
3. Run a model (so it’s loaded and ready):
```
ollama run llama3.1:latest
```

4. create a new file named .env,In the root folder of the repo (hello-genai/).
```
LLM_BASE_URL=http://localhost:11434
LLM_MODEL_NAME=llama3.1:latest

```

5. Run the stack:
```
docker compose up
```
Create and runs multiple containers
```
docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS                   PORTS                                         NAMES
5b0ad049562b   hello-genai-python-genai   "python app.py"          2 minutes ago   Up 2 minutes (healthy)   0.0.0.0:8081->8080/tcp, [::]:8081->8080/tcp   hello-genai-python-genai-1
f664b1a7b890   hello-genai-node-genai     "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes (healthy)   0.0.0.0:8082->8080/tcp, [::]:8082->8080/tcp   hello-genai-node-genai-1
ab00076f7f2e   hello-genai-go-genai       "./main"                 2 minutes ago   Up 2 minutes (healthy)   0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp   hello-genai-go-genai-1
8be20d0692a9   hello-genai-rust-genai     "./rust-genai"           2 minutes ago   Up 2 minutes (healthy)   0.0.0.0:8083->8080/tcp, [::]:8083->8080/tcp   hello-genai-rust-genai-1
```
6. Run the stack
```
docker compose up

```


