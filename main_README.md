
# 🧠 In-Depth Chainlit Agent Setup Guide

Welcome to the **detailed walkthrough** for building and running your first OpenAI Agent using [Chainlit](https://docs.chainlit.io/). This guide expands upon the quick steps and provides more clarity for each stage.

## 📦 Project Initialization

Begin by creating a new project using the `uv` tool:

```bash
uv init --package hello_op_ag
cd hello_op_ag
code .
```

## ⚙️ Install Required Packages

Add the OpenAI Agents SDK and Chainlit dependencies:

```bash
uv add openai_agents
uv add chainlit
```

## 🚀 Run & Setup Environment

Run the initialized environment:

```bash
uv run hello_op_ag
```

To test Chainlit installation:

```bash
uv run chainlit hello
```

## 🤖 Create a Chainlit Agent

In your project directory (`src/hello_op_ag`), create a file named `chatbot.py` and add the following:

```python
import chainlit as cl

@cl.on_message
async def main(message: cl.Message):
    await cl.Message(content=f"Received: {message.content}").send()
```

## ▶️ Launch Your Chatbot

```bash
cd src/hello_op_ag
uv run chainlit run chatbot.py -w
```

The chatbot will be available at [http://localhost:8000](http://localhost:8000).

## ✅ Best Practices

- Use `@cl.on_message` to handle user input.
- Keep your agent logic modular.
- Use `-w` for auto-reloading.

---

This is a companion file to the main [README](../README.md). Use this when you need a slower, step-by-step walkthrough.
