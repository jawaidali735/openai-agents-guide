Yeh README file aapko samjhayegi ke kaise Runner.run_streamed() aur stream_events() ka istemal karte hain real-time token streaming ke liye — jaise ChatGPT responses appear karte hain.

🧩 Highlighted Code Block
python
Copy
Edit
async for event in result.stream_events():
    if event.type == "raw_response_event" and isinstance(event.data, ResponseTextDeltaEvent):
        await msg.stream_token(event.data.delta)
🎯 Pehle Context Samjho
Runner.run_streamed() ek streaming process start karta hai jisme agent ka response token-by-token milta hai — bilkul ChatGPT jaisa experience.

stream_events() ek async generator hai jo AI model se har naye token ka event real-time mein bhejta hai.

🧠 Line-by-Line Breakdown
🔹 async for event in result.stream_events():
python
Copy
Edit
async for event in result.stream_events():
Yeh ek asynchronous loop hai jo har naye token ke event ko await karke receive karta hai.

🔹 if event.type == "raw_response_event":
python
Copy
Edit
if event.type == "raw_response_event":
Har event ka ek type hota hai. Yeh check karta hai ke kya event ka type raw_response_event hai — yani ke actual token ka event aya hai.

🔹 isinstance(event.data, ResponseTextDeltaEvent)
python
Copy
Edit
isinstance(event.data, ResponseTextDeltaEvent)
Yeh check karta hai ke event.data ka type ResponseTextDeltaEvent hai ya nahi — jo streamed token ko represent karta hai.

🔹 await msg.stream_token(event.data.delta)
python
Copy
Edit
await msg.stream_token(event.data.delta)
event.data.delta mein ek naya word/token hota hai. msg.stream_token() use user interface (chat bubble) mein real-time show karta hai.

💬 Visual Flow:
text
Copy
Edit
User ➜ Message ➜ Agent ➜ Streaming Response ➜ Token ➜ Shown in UI
📜 Urdu mein Samajh Lo:
"Jab tak AI ka jawaab aa raha hai, har naye word/token ke liye dekho. Agar event ka type raw_response_event hai aur woh ek ResponseTextDeltaEvent hai, to us token ko foran user ko screen par dikhao — jaise ke type ho raha ho."

🧾 Summary Table
Part	Explanation
async for event in result.stream_events()	Token stream ko async loop mein read karta hai
event.type == "raw_response_event"	Check karta hai ke event actual token hai
ResponseTextDeltaEvent	Streamed token ka structure hota hai
msg.stream_token(...)	Token ko UI par type hota hua show karta hai
