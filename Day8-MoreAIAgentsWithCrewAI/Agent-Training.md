## Agent-Training
> crew.py  --> @training_handler
it refers to CrewAI’s built-in Agent Training feature.

This isn't traditional machine learning model training (you aren't fine-tuning the weights of Llama 3 or GPT-4). Instead, CrewAI training is Prompt Optimization via Reinforcement Learning. It runs your crew multiple times, tracks the feedback you provide on their outputs, and saves those corrections to a local training file (training_data.pkl). The next time the crew runs, it injects these past lessons into the agents' prompts so they don't repeat mistakes.


Here is a guide on how much you should train your agents for a production DevOps use case like your Internal Documentation analyzer.
🔢 The Rule of Thumb: How Many Iterations?
When you execute the training command:

```Bash
crewai train -n <number_of_iterations>
```
The value of -n depends entirely on what you are trying to fix:

##### 1. Simple Task Alignment: 3 to 5 Iterations
- When to use: Your agents are mostly doing a good job, but they occasionally miss a specific formatting rule, include unnecessary conversational filler ("Sure, here is your summary..."), or use the wrong tone.
- Why: It only takes a few examples of you telling the agent "Do not include conversational filler" or "Format this output as a strict markdown table" for the framework to lock in that behavior.

#### 2. Complex Logic & Tool Routing: 10 to 15 Iterations
- When to use: Your agents are getting confused about which tool to use (e.g., they keep using a broad web search tool when they should be querying your local PDF database tool), or they are getting stuck in repetitive loops.
- Why: The orchestrator needs to map out multiple diverse scenarios to learn exactly when a specific action or tool execution is appropriate.

###### 🛑 Warning on Over-Training (15+ Iterations): > Do not set your iterations too high (like 30 or 50). Because local LLMs have limited context windows, over-training can cause Overfitting. Your agents will become too rigid, memorizing your exact training examples, and they will fail or hallucinate when a user asks a slightly different question in production.
#### 🛠️ The Local LLM Training Reality Check
Since you are setting up this project to run on a Local LLM (via Ollama), keep this critical technical limitation in mind for your UC1 guide:
- The Hardware Tax: Training requires running your entire crew's workflow from start to finish $N$ times consecutively. If one full run takes 2 minutes on your local machine, training for 10 iterations will take 20 minutes of continuous, heavy CPU/GPU utilization.
- Model Quality Matters: Smaller local models (like llama3:8b or mistral:7b) sometimes struggle to self-correct during training cycles compared to massive cloud models.
