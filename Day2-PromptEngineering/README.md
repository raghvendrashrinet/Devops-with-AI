# Mastering Prompt Engineering for DevOps

Prompt engineering is the art of crafting instructions to get the most accurate and helpful responses from AI models. In this guide, you'll learn how to master prompt engineering using the following techniques, with DevOps-related examples:
## 📌 Quick Keyword Reference Guide

Use this matrix to instantly decide which prompting technique fits your current task:

| Technique | When to Use | DevOps Example Case | Efficiency |
| :--- | :--- | :--- | :--- |
| **Zero-Shot** | Simple, standard tasks with no unique context. | Writing basic Bash loops or generic Git commands. | 🚀 Fast & Direct |
| **Few-Shot** | Custom syntax, strict formatting, or specific tool outputs. | Matching a proprietary log format or specific JSON output. | 📐 Structured |
| **Multi-Shot** | Complex multi-command sequences or behavioral patterns. | Writing complex multi-stage Jenkinsfiles or GitLab CI. | 🤖 High Accuracy |
| **Chain-of-Thought (CoT)** | Debugging errors, root-cause analysis, architecture design. | Troubleshooting a Kubernetes `CrashLoopBackOff` or IAM block. | 🧠 Deep Reasoning |

---
## Zero-Shot Prompting

In zero-shot prompting, You ask the AI to perform a task relying solely on its pre-trained knowledge, without providing any examples.

Example 1: Generate a Shell Script to Backup Logs

```
Prompt:

Write a shell script to back up log files from /var/logs to /backup.

Response:

#!/bin/bash
# Backup logs from /var/logs to /backup
mkdir -p /backup
cp -r /var/logs/* /backup/
echo "Backup completed!"
```

Example 2: Explain the Purpose of a kubectl get pods Command

```
Prompt:

Explain the purpose of the kubectl get pods command.

Response:

The kubectl get pods command lists all the running and pending pods in the current Kubernetes namespace. It provides information about the pod name, status, restart count, and age.
```

### Few-Shot Prompting

You provide **one or two examples** of the expected input and output format before asking the final question. This aligns the AI to your specific coding standards.

Example 1: Write a Shell Script to Create and Archive Logs

```
Prompt:

Here are some shell scripts to manage logs:

Example 1:

# Create a log file
touch /var/logs/app.log
echo "Application started" > /var/logs/app.log

Example 2:

# Archive old logs
tar -czf /backup/app_logs.tar.gz /var/logs/*

Now write a script to delete logs older than 7 days.

Response:

#!/bin/bash
# Delete logs older than 7 days
find /var/logs -type f -mtime +7 -exec rm {} \;
echo "Old logs deleted!"
```

### Multi-Shot Prompting

Multi-shot prompting is an extension of few-shot prompting where you provide multiple examples and progressively refine the task.

Example 1: Create a Kubernetes Deployment and Describe It

```
Prompt:

Example 1:

kubectl create deployment nginx --image=nginx

Example 2:

kubectl get deployment nginx

Example 3:

kubectl describe deployment nginx

Now write a command to scale the nginx deployment to 5 replicas.

Response:

kubectl scale deployment nginx --replicas=5
```

### Chain of Thought (CoT) Prompting

Forces the AI to break down complex logic **step-by-step** before spitting out code. This drastically reduces AI hallucinations in complex infrastructure environments.
* **Best for:** Incidents, performance bottlenecks, and tricky network routing issues.
* **Prompt:**
    > "Our microservice is experiencing intermittent 504 Gateway Timeouts behind an AWS ALB. Walk through a step-by-step troubleshooting workflow to isolate if the issue is at the ALB, the Nginx reverse proxy, or the application layer."
---

## 💡 Pro-Tips to Master DevOps Prompting
### 🛡️ 1. Use System Role Prompting (The "Act As" Trick)
Always prime the AI by giving it a specific persona. It shifts the AI's internal weights to prioritize engineering-grade outputs over generic answers.
* *Bad:* "How do I secure docker?"
* *Good:* `"Act as a Principal DevSecOps Engineer. Audit the following Dockerfile for security vulnerabilities, focusing on rootless execution, multi-stage builds, and credential leaks..."`

### 🏗️ 2. Apply the "Context-Constraint-Output" (CCO) Framework
Structure your prompts using this mental model to ensure you never get junk output:
* **Context:** What is the current environment? (e.g., *"We are migrating from Jenkins to GitHub Actions..."*)
* **Constraint:** What are the boundaries? (e.g., *"Do not use third-party marketplace actions; use raw shell commands only..."*)
* **Output:** What format do you need? (e.g., *"Provide only the valid YAML block, no conversational text."*)

### 🔍 3. The "Self-Correction" Loop
Before running an AI-generated script in production, ask the model to review its own work.
* **Follow-up Prompt:** `"Review the script you just generated. Identify any edge cases where it might fail (e.g., missing environment variables, empty directories, or network timeouts) and rewrite it to handle those gracefully."`

### 🛑 4. Never Share Secrets
* **Rule of Thumb:** Never paste raw `.env` files, production Kubeconfigs, AWS IAM secret keys, or proprietary private keys into public LLMs. Use placeholders like `<AWS_ACCOUNT_ID>` or `your-api-key-here`.
  
✅ Best Practices for Prompt Engineering

Be clear and specific – The more specific the prompt, the better the output.

Use context – Provide background information or examples when needed.

Iterate and refine – If the output isn’t ideal, adjust the prompt.

Use CoT for complex tasks – Step-by-step reasoning improves accuracy.
