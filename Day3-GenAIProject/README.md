
##  What is an LLM?
- **Definition**: A Large Language Model (LLM) is a machine learning model trained on massive text datasets to understand and generate human-like language.
- **Core capabilities**:
  - Text generation
  - Summarization
  - Translation
  - Question answering
  - Code assistance
---    
##  LLM Providers
- **OpenAI** → GPT models (GPT-4, GPT-3.5)  
- **Anthropic** → Claude models  
- **Google DeepMind** → Gemini  
- **Meta** → LLaMA family  
- **Mistral AI** → Lightweight performant models  
- **Cohere** → Enterprise NLP APIs  
- **Microsoft Azure OpenAI Service** → Hosted GPT models with enterprise integration  
---
## 🏗️ Hosted vs Local LLMs

| Aspect              | Hosted LLMs (Cloud)                          | Local LLMs (On-Prem / Self-hosted)         |
|---------------------|-----------------------------------------------|--------------------------------------------|
| **Setup**           | Minimal, managed by provider                  | Complex, requires infra + GPU/TPU          |
| **Scalability**     | Auto-scaled by provider                       | Limited by local hardware                  |
| **Cost**            | Pay-per-use / subscription                    | Hardware investment, but lower long-term   |
| **Latency**         | Network-dependent                             | Faster (local inference)                   |
| **Security**        | Data leaves org (risk)                        | Full control, data stays in-house          |
| **Use cases**       | Quick prototyping, SaaS apps                  | Enterprise apps, privacy-sensitive domains |

---
---
## 📋 Setting Up OLLAMA Locally- Prerequisites

### Installing Ollama

1. **Download and Install Ollama**
   ```bash
   # For Linux
   curl -fsSL https://ollama.com/install.sh | sh

   # For MacOS
   brew install ollama

   # For Windows
   # Download the installer from https://ollama.com/download/windows
   # Or use Windows Package Manager:
   winget install Ollama.Ollama
   ```

2. **Start Ollama Service**
   ```bash
   # For Linux and MacOS
   ollama serve

   # For Windows
   # Ollama runs as a service automatically after installation
   # You can access it at http://localhost:11434
   ```

3. **Pull Llama3 Model**
   ```bash
   ollama pull llama3.2:1b
   ```
4. **Verify Installation**
   ```
   # Test Ollama is running by pulling a small model
     ollama pull tinyllama

   # Run a quick test
     ollama run tinyllama "Hello, how are you?"
 
  ## Python Project Overview: How to integrate LLMs into Python projects.
  Project Objective : Generate a dockerfile for user provided programing language
  - Demonstrate Local vs Hosted LLM Integration
```
├── ├── LocalLLM/
│   |     ├── Ollama/
├── ├── Hosted LLM/
          ├── Gemini
```
│   ├── local_llm.py 
