##  AI Agent for your Internal Documentation
- This project will use Meta Quest knowledge repo example
### PDF Knowledge Example
This project demonstrates how to create a Crew of AI agents and tasks using crewAI. It uses a PDF knowledge source to answer user questions based on the content of the PDF. The PDF is loaded from a file and the knowledge source is initialized with it. The project also includes a custom task that uses the knowledge source to answer user questions. You can modify the question in the main.py file.

 
### Clone the repo
 **Clone the repository and navigate to the project directory:**
   ```bash
   git clone [https://github.com/raghvendrashrinet/Devops-with-AI.git](https://github.com/raghvendrashrinet/Devops-with-AI.git)
   cd Devops-with-AI/Day8-MoreAIAgentsWithCrewAI/meta_quest_knowledge
```
### Installation
> pip install uv
> crewai install
or
> pip3 install crewai

### Customizing
1.Add your OPENAI_API_KEY into the .env file
For using OpenAI : 
```
- Modify src/meta_quest_knowledge/config/agents.yaml to define your agents
- Modify src/meta_quest_knowledge/config/tasks.yaml to define your tasks
- Modify src/meta_quest_knowledge/crew.py to add your own logic, tools and specific args
- Modify src/meta_quest_knowledge/main.py to add custom inputs for your agents and tasks
```
2. For Using Local LLM Installed on the System
- Add these lines to your .env file: in the root directory 
```
MODEL=ollama/llama3
OPENAI_API_BASE=http://localhost:11434/v1
OPENAI_API_KEY=ollama
```
3. Update your crew.py file
Inside your src/meta_quest_knowledge/crew.py file, you need to import the LLM configuration and explicitly pass your local model to the agents.
```
local_llm = LLM(
        model=os.environ.get("MODEL", "ollama/llama3"),
        base_url=os.environ.get("OPENAI_API_BASE", "http://localhost:11434/v1")
    )
```

Here is how to modify your crew.py structure:

### Running the project
```
$ crewai run
```
