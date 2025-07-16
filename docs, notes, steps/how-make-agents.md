# 🚗 Chainlit Car Leasing Assistant (Streaming Agent) – Urdu Documentation


---

## 📦 Requirements

Ensure your environment has the following:

```bash
pip install chainlit openai python-dotenv
```

---

## 🔑 Environment Setup

`.env` file mein apna Gemini API key daalein:

```
GEMINI_API_KEY=your_gemini_api_key_here
```

---

## 🧠 Core Concepts Used

- Chainlit Chat UI
- Streaming token-by-token response
- Agent logic for leasing cars only
- AsyncOpenAI with Gemini API
- Session-based chat history

---

## 🧱 Full Code (With Urdu Explanation Below Each Block)

### 1. 🔄 Imports & Environment Load

```python
import chainlit as cl
from agents import Agent, AsyncOpenAI, OpenAIChatCompletionsModel, RunConfig, Runner
from dotenv import find_dotenv, load_dotenv
import os

load_dotenv(find_dotenv())
from openai.types.responses import ResponseTextDeltaEvent
```

📘 *Ye sab imports Chainlit + Agents SDK + dotenv ke liye hain. Dotenv env file se API key uthata hai.*

---

### 2. 🔐 Gemini API Configuration

```python
api_key = os.getenv("GEMINI_API_KEY")

external_client = AsyncOpenAI(
    api_key=api_key,
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
)
```

📘 *Yahan `AsyncOpenAI` ka istemal ho raha hai Gemini API ko use karne ke liye (OpenAI compatible format).*

---

### 3. 🧠 Model & RunConfig Setup

```python
model = OpenAIChatCompletionsModel(
    model="gemini-2.0-flash",
    openai_client=external_client
)

config = RunConfig(
    model=model,
    tracing_disabled=True,
    model_provider=external_client
)
```

📘 *Model define kiya gaya hai (Gemini flash variant) aur uske config ke through tracing disable ki gayi hai.*

---

### 4. 👨‍💼 Agent Creation

```python
agent = Agent(
    name="Assitant leasaing Car ",
    instructions="You are Assistant leasing car companies like BMW, Mercedes and so on and you are only an assistant of leasing cars. You don't have to answer irrelevant questions.",
)
```

📘 *Yeh agent strictly leasing-related sawaalon ke liye banaya gaya hai — irrelevant sawaal ka jawab nahi dega.*

---

### 5. 💬 Initial Chat Start Event

```python
@cl.on_chat_start
async def handle_chat_history():
    cl.user_session.set("history", [])
    await cl.Message(content="Assistant of leasing cars, How can I help you").send()
```

📘 *Chat start hone par user history ko empty list se initialize karta hai aur welcome message bhejta hai.*

---

### 6. 💬 Message Handler With Streaming

```python
@cl.on_message
async def receive_message(message: cl.Message):
    history = cl.user_session.get("history")
    msg = cl.Message(content="")
    await msg.send()

    history.append({"role": "user", "content": message.content})

    result = Runner.run_streamed(
        agent,
        input=history,
        run_config=config
    )

    async for event in result.stream_events():
        if event.type == "raw_response_event" and isinstance(event.data, ResponseTextDeltaEvent):
            await msg.stream_token(event.data.delta)

    history.append({"role": "assistant", "content": result.final_output})
    cl.user_session.set("history", history)
```

📘 *Yahan har user message ke liye:
- `history` update hoti hai
- `Runner.run_streamed` se AI response aata hai
- Har naya token streaming se dikhaya jata hai (line-by-line like typing)
- Final response ko history mein store kiya jata hai*

---

## 🎯 Summary

| Feature | Description |
|--------|-------------|
| `cl.on_chat_start` | User session start & greet message |
| `Agent` | Leasing assistant agent |
| `Runner.run_streamed` | Real-time streamed token response |
| `ResponseTextDeltaEvent` | Each new piece of answer shown live |
| `user_session['history']` | Context maintained across messages |

---

## 🏁 Run the App

```bash
chainlit run app.py -w
```

✅ App `localhost:8000` par chalega jahan aap leasing assistant se baat kar sakte ho.

---

## 🔐 Note

Assistant sirf car leasing related queries ka answer karega. Agar user irrelevant ya off-topic sawaal kare, toh agent politely refuse karega.

---

