## Day 7: Docker Model Runner(DMR)

Welcome to Day 7! Today's focus is containerizing our AI environment using **Docker**. Instead of installing dependencies directly on your host machine, we will package our local LLM runner into an isolated Docker container. This ensures consistency across development, testing, and production environments.

---

## Prerequisites & Docker Setup

Before running an LLM inside a container, you need Docker installed on your machine.

### 1. Install Docker Engine / Desktop
* **Linux (Ubuntu):**
  ```bash
  sudo apt-get update
  sudo apt-get install docker.io -y
  sudo systemctl start docker
  sudo systemctl enable docker

  On Windows : Download Docker Desktop
  Download the installer (Docker Desktop Installer.exe).
  
### 2. Verify Your Installation
```bash
docker --version
docker run hello-world
```
---
1. Downloading model from docker hub : eg small llm model smollmm2
```bash
docker model pull ai/smollm2:135M-Q4_K_M
```
2. Once pulled locally, check
```bash
docker model list
MODEL NAME           PARAMETERS  QUANTIZATION    ARCHITECTURE  MODEL ID      CREATED       CONTEXT  SIZE
smollm2:135M-Q4_K_M  134.52 M    IQ2_XXS/Q4_K_M  llama         d2df8c834967  8 months ago     8192  98.87 MiB
```
3. Run query to the model
```
 docker model run ai/smollm2:135M-Q4_K_M
 > Send a message (/? for help)
```

