
# Chainlit History Handling (Roman Urdu mein Samjhaya gaya)

## 🧠 cl.user_session.set("history", [])

Yeh line:
```python
cl.user_session.set("history", [])
```
Chainlit ke session mein ek khaali list save karti hai "history" key ke andar.

**Matlab:** Har naya user jab chat start karega, uske liye ek fresh history banayi jaayegi jisme koi purani messages nahi honge.

🧠**Samjho:**
Har user ke liye ek temporary memory (dictionary) hoti hai — cl.user_session — jismein aap kuch bhi save kar sakte ho: messages, tokens, ID, etc.

Yeh line:
```python
cl.user_session = {
    "history": []
}
```
---

## 🔁 Messages ka Format aur Flow

Jab user kuch likhta hai:

```python
history.append({"role": "user", "content": message.content})
```

Aur jab AI jawab deta hai:

```python
history.append({"role": "assistant", "content": result.final_output})
```

Phir yeh updated list dobara session mein save ki jaati hai:

```python
cl.user_session.set("history", history)
```

---

## 👁️ Visual Format

```python
[
    {"role": "user", "content": "Hi"},
    {"role": "assistant", "content": "Hello! How can I help you?"},
    {"role": "user", "content": "Python kya hai?"},
    {"role": "assistant", "content": "Python aik programming language hai..."}
]
```

---

## 💬 AI ko Kaise Samajh aata hai?

AI ko poori history di jaati hai jab hum `Runner.run(...)` karte hain:

```python
result = await Runner.run(
    agent,
    input=history,
    run_config=run_config
)
```

AI last user message ko read karta hai, lekin pehle ke context se bhi samajh leta hai ke baat kaise chal rahi hai.

---

## ❓ cl.user_session.set ka Faida kya hai?

Jab aap `append()` karte ho, wo sirf Python list ko update karta hai. Chainlit ko nahi pata chalta ke aap ne list change ki hai. Isliye:

```python
cl.user_session.set("history", history)
```

yeh line zaroori hai — takay updated list session mein properly save ho jaaye.

---

## ✅ Summary Table

| Concept | Explanation |
|--------|-------------|
| `cl.user_session` | Chainlit ka session object har user ke liye |
| `"history"` key | Ek list jisme user aur assistant ke sare messages hote hain |
| `append()` | Har new message ko list mein add karta hai |
| `set()` | Updated list ko session mein dobara save karta hai |
| `input = history` | AI ko full chat history di jaati hai |

---

Agar aapko isko PDF format mein chahiye ya aur koi example chahiye to zaroor batayein.
