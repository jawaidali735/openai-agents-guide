
# 📘 Intro to OpenAI Agents SDK with Chainlit

This guide will walk you through setting up an OpenAI Agents SDK project with Chainlit UI. Ideal for beginners and those exploring agents with interactive chat interfaces.

## ✅ Prerequisites

- Python installed
- [`uv` CLI tool](https://github.com/astral-sh/uv) installed (alternative to pip & venv)
- Code editor (like VS Code)

## 🚀 Step-by-Step Guide

### 🔹 Step 1: Initialize a New Project

```bash
uv init --package hello_op_ag
cd hello_op_ag
code .
```

> `hello_op_ag` is your project folder name. You can choose any name.

### 🔹 Step 2: Run Initial Project Setup

```bash
uv run hello_op_ag
```

> This sets up the environment and prepares your project structure.

### 🔹 Step 3: Add OpenAI Agents SDK

```bash
uv add openai_agents
```

### 🔹 Step 4: Add Chainlit

```bash
uv add chainlit
```

### 🔹 Step 5: Test Chainlit Installation

```bash
uv run chainlit hello
```

> This runs a sample Chainlit interface. Make sure it's working before proceeding.

## 🧠 Create Your First Chainlit Agent

1. Inside your project, go to the `src/hello_op_ag/` folder.
2. Create a new file named `chatbot.py`.

Paste the following code inside it:

```python
import chainlit as cl

@cl.on_message
async def main(message: cl.Message):
    # Custom logic can go here (API calls, prompts, etc.)
    await cl.Message(
        content=f"Received: {message.content}",
    ).send()
```

## ▶️ Run Your Chatbot

From the `src/hello_op_ag/` folder, run:

```bash
uv run chainlit run chatbot.py -w
```

> `-w` enables auto-reloading when you update your code.

## 📝 Summary of Commands

```bash
uv init --package hello_op_ag
cd hello_op_ag
code .

uv run hello_op_ag

uv add openai_agents
uv add chainlit

uv run chainlit hello

# After adding chatbot.py
cd src/hello_op_ag
uv run chainlit run chatbot.py -w
```

## 📄 Useful Notes

- Use `@cl.on_message` for replying to user input.
- You can also use `@cl.on_chat_start` if you want to send a welcome message.
- Chainlit runs at: [http://localhost:8000](http://localhost:8000)

## 🧾 Example File Tree

```
hello_op_ag/
├── pyproject.toml
└── src/
    └── hello_op_ag/
        └── chatbot.py
```
