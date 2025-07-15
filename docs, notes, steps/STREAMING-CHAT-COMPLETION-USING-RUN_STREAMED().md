Streaming Chat Completion using run_streamed()
🔁 Yeh Part:
python
Copy
Edit
async for event in result.stream_events():
    if event.type == "raw_response_event" and isinstance(event.data, ResponseTextDeltaEvent):
        await msg.stream_token(event.data.delta)
🎯 Pehle Context Samjho
Runner.run_streamed() ek streaming process start karta hai jisme agent ka response thoda thoda karke (token by token) milta hai — jaise ChatGPT web mein typing dikhai deti hai.

Ye response ek stream_events() naam ka async generator deta hai jo har event deta hai jab bhi kuch new token ya data mile.

🧠 Breakdown:
🔸 async for event in result.stream_events():
Yeh ek asynchronous loop hai jo real-time mein har response token ka event receive karta hai.

Har event mein kuch information hoti hai (jaise naya token aaya, ya response complete hua, etc.)

🔸 if event.type == "raw_response_event"
Har event ka ek type hota hai.

raw_response_event ka matlab hai ke AI model ka raw (original) token/part of answer aaya hai.

🔸 isinstance(event.data, ResponseTextDeltaEvent)
Yeh check karta hai ke event.data ka type hai ResponseTextDeltaEvent ya nahi.

ResponseTextDeltaEvent ek special class hoti hai jo streamed text token ko represent karti hai.

Iska use tab hota hai jab response real-time mein slowly arrive kar raha ho.

🔸 await msg.stream_token(event.data.delta)
event.data.delta mein ek naya token/text character hota hai jo abhi model ne bheja.

msg.stream_token() us nayi line/word/token ko user UI mein live show karta hai (jaise chat bubbles type hoti hain).

💬 Visual Flow:
Aap user ka message bhejte ho

Agent response generate karta hai streamed mode mein

Har token event ke form mein aata hai (type: raw_response_event)

Har naya token screen par type hota hua dikhai deta hai

✅ Poora Block Samjho in Easy Urdu:
python
Copy
Edit
async for event in result.stream_events():
    if event.type == "raw_response_event" and isinstance(event.data, ResponseTextDeltaEvent):
        await msg.stream_token(event.data.delta)
📜 Urdu mein matlab:
"Jab tak AI ka jawaab aa raha hai, har naye word/token ke liye dekho. Agar event ka type 'raw_response_event' hai aur woh ek text-type response hai, to us token ko turant user ke samne display karo (jaise type ho raha ho)."

🔚 Summary (Samjhne ke Liye):
Part	Explanation
async for event in ...	Live event listener for streamed AI response
event.type == "raw_response_event"	Check kar raha hai ke naya response token aya hai
isinstance(..., ResponseTextDeltaEvent)	Check kar raha hai ke woh event text token hai ya nahi
await msg.stream_token(...)	Naya token chat UI mein live stream (type hota hua) dikhana
