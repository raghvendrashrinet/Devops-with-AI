1. Ollam installed
```
list ollam models installed 
 > ollama ls

install ollam
  > ollama install  llama3.1
  > ollama run llama3.1
```
2. Always Create python env to separate dependency 
```
    python -m venv crew
    .\crew\Scripts\activate
    deactivate --> to come out of env
```
3. Install CrewAI
```
  pip3 install crewai
  or
  crewai install
```

4. Create new Crewai project
```
  > crewai create crew devops-project
     Select a provider out of list : eg 7 ollama
     Select a model to use for Ollama:  1  --> for ollama/llama3.1
```
It is used to scaffold a new CrewAI project

### What Happens
1. Creates a new project folder
- A directory named devops-project is generated.
- This becomes the workspace for your crew (multi‑agent system).
2.Generates starter files  
  Inside my_project/, you’ll typically see:
  - agents.yaml → defines your agents (roles, tools, personalities).
  - tasks.yaml → defines tasks those agents will perform.
  - crew.py → Python entry point to orchestrate agents and tasks.
  - pyproject.toml → project metadata and dependencies.
3. Sets up configuration
  - Links agents and tasks together into a “crew.”
  - Prepares the project so you can immediately run it with:
  ```
  cd devops-project
  crewai run
  ```
  4. Provides a boilerplate structure
  - You don’t start from scratch — you get a ready‑made template to customize.
  - Makes it easy to define workflows like “Agent A generates pipeline YAML → Agent B validates → Agent C commits to Git.”
  
  5. Generates MD file output
---
### configuration files:
1. config/agents.yaml: This is where you define the identities of your agents. The default template fills this with placeholders for roles, goals, and backstories.
2. config/tasks.yaml: This defines the specific assignments. Each task explicitly points to an agent defined in the previous file.
3. crew.py: This is the orchestrator file. It uses Python decorators (like @agent and @task) to read your YAML configurations, instantiate the components, and assemble them into a cohesive Crew()
