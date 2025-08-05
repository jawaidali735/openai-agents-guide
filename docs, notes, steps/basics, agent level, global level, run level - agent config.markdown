# 05_model_configuration

## Mukhtasar Jaan Pehchaan
Yeh folder `learn-agentic-ai` repository ke `01_ai_agents_first` module ka hissa hai aur **OpenAI Agents SDK** ke zariye **Agentic AI** ke model configuration ke teen mukhtalif levels ko samjhaata hai: **Agent Level**, **Run Level**, aur **Global Level**. Yeh folder batata hai ke kaise aap default OpenAI provider ke bajaye koi aur LLM provider (jaise Google Gemini ya OpenRouter) set kar sakte hain aur apne agents ke liye mukhtalif settings configure kar sakte hain.

## Maqsad
Is folder ka maqsad yeh hai ke aap samajh saken:
- Har agent ke liye alag model aur provider kaise set karna (**Agent Level**).
- Run-time pe model aur provider kaise define karna (**Run Level**).
- Puri application ke liye default provider aur settings kaise set karna (**Global Level**).
- In teen levels ki priority aur unka practical use.

## Zaroori Cheezein
- **Python 3.8+** installed hona chahiye.
- **OpenAI Agents SDK** aur dependencies install karni hongi:
  ```bash
  pip install openai-agents
  ```
- Ek API key zaroori hai (jaise Google Gemini ya OpenRouter ke liye). API key hasil karne ke liye:
  - Google Gemini: [Google AI Studio](https://ai.google.dev)
  - OpenRouter: [OpenRouter Dashboard](https://openrouter.ai)
- Python, async programming, aur APIs ki basic samajh.

## Folder Structure
- `agent_level.py`: Agent Level configuration ka example code.
- `run_level.py`: Run Level configuration ka example code.
- `global_level.py`: Global Level configuration ka example code.

## Configuration ke Teen Levels

### Background
OpenAI Agents SDK default taur par OpenAI ke models (jaise GPT-4) ke liye set hota hai. Lekin agar aap koi aur LLM provider (jaise Google Gemini ya OpenRouter) use karna chahte hain, toh aapko configuration set karni hoti hai. Yeh configuration teen levels pe ki ja sakti hai: **Agent Level**, **Run Level**, aur **Global Level**.

#### 1. Agent Level
**Kya Hai?** Is level pe aap har **Agent** ke liye alag se ek custom LLM provider aur model set karte hain. Yani, har agent apne hisaab se ek specific model use kar sakta hai.  
**Kab Use Karna?** Jab aap chahte hain ke har agent apna alag model ya provider use kare (masalan, ek agent Gemini, doosra OpenAI, teesra OpenRouter).  
**Fayda**: Har agent ke liye flexibility milti hai; aap alag-alag agents ke liye alag instructions aur models set kar sakte hain.  
**Example**:
```python
import asyncio
from openai import AsyncOpenAI
from agents import Agent, OpenAIChatCompletionsModel, Runner, set_tracing_disabled

gemini_api_key = "your_api_key_here"
client = AsyncOpenAI(
    api_key=gemini_api_key,
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
)
set_tracing_disabled(disabled=True)

async def main():
    agent = Agent(
        name="Assistant",
        instructions="You only respond in haikus.",
        model=OpenAIChatCompletionsModel(model="gemini-2.0-flash", openai_client=client),
    )
    result = await Runner.run(agent, "Tell me about recursion in programming.")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```
**Samjhaye**:
- Ek `AsyncOpenAI` client banaya jata hai jo Google Gemini ke API se connect hota hai (`base_url` aur `gemini_api_key` ke saath).
- `set_tracing_disabled(True)` se debugging traces band kiye jate hain.
- `Agent` object banaya jata hai aur usmein `OpenAIChatCompletionsModel` ke saath Gemini ka `gemini-2.0-flash` model set kiya jata hai.
- Agent ko instruction di jati hai ke woh sirf haikus mein jawab de.
- `Runner.run` ke zariye agent ek query ("Tell me about recursion in programming") ke saath chalaya jata hai.
**File**: `agent_level.py`

#### 2. Run Level
**Kya Hai?** Is level pe aap model aur provider ko **run-time** pe define karte hain, yani jab agent ko chalate hain tab. Yeh agent ke creation se alag hota hai aur har run ke liye specific settings allow karta hai.  
**Kab Use Karna?** Jab aap chahte hain ke ek agent alag-alag runs ke liye mukhtalif models ya providers use kare (masalan, ek run mein Gemini, doosre mein OpenAI).  
**Fayda**: Run-time pe flexibility milti hai; aap ek agent ko multiple providers ke saath test kar sakte hain bina agent ki definition change kiye.  
**Example**:
```python
from agents import Agent, Runner, AsyncOpenAI, OpenAIChatCompletionsModel
from agents.run import RunConfig

gemini_api_key = "your_api_key_here"
external_client = AsyncOpenAI(
    api_key=gemini_api_key,
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
)
model = OpenAIChatCompletionsModel(model="gemini-2.0-flash", openai_client=external_client)
config = RunConfig(model=model, model_provider=external_client, tracing_disabled=True)

agent = Agent(name="Assistant", instructions="You are a helpful assistant")
result = Runner.run_sync(agent, "Hello, how are you.", run_config=config)
print(result.final_output)
```
**Samjhaye**:
- Ek `AsyncOpenAI` client banaya jata hai jo Gemini ke API se connect hota hai.
- `OpenAIChatCompletionsModel` ke saath `gemini-2.0-flash` model set kiya jata hai.
- `RunConfig` object banaya jata hai jismein model, provider (`external_client`), aur settings (jaise `tracing_disabled=True`) define kiye jate hain.
- `Agent` banaya jata hai bina kisi model ke (default settings ke saath).
- `Runner.run_sync` chalane pe `RunConfig` ke zariye model aur provider specify kiye jate hain.
- Query ("Hello, how are you.") ke jawab mein agent Gemini model use karta hai.
**File**: `run_level.py`

#### 3. Global Level
**Kya Hai?** Is level pe aap **puri application** ke liye ek default provider aur settings set karte hain. Yeh settings sab agents ke liye apply hoti hain, jab tak Agent Level ya Run Level pe kuch aur specify na kiya jaye.  
**Kab Use Karna?** Jab aap chahte hain ke sab agents ek hi provider aur settings use karen, bina har agent ya run ke liye alag se configure kiye.  
**Fayda**: Code simple ho jata hai kyunki aapko har agent ya run ke liye provider set nahi karna padta; default settings puri app ke liye consistent hoti hain.  
**Example**:
```python
from agents import Agent, Runner, AsyncOpenAI, set_default_openai_client, set_tracing_disabled, set_default_openai_api

gemini_api_key = "your_api_key_here"
set_tracing_disabled(True)
set_default_openai_api("chat_completions")
external_client = AsyncOpenAI(
    api_key=gemini_api_key,
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
)
set_default_openai_client(external_client)

agent = Agent(name="Assistant", instructions="You are a helpful assistant", model="gemini-2.0-flash")
result = Runner.run_sync(agent, "Hello")
print(result.final_output)
```
**Samjhaye**:
- Globally `set_tracing_disabled(True)` se tracing band kiya jata hai.
- `set_default_openai_api("chat_completions")` se default API type set kiya jata hai (OpenAI-compatible Chat Completions API).
- `AsyncOpenAI` client banaya jata hai jo Gemini ke API se connect hota hai aur `set_default_openai_client(external_client)` se isse default provider banaya jata hai.
- `Agent` banaya jata hai aur usmein `model="gemini-2.0-flash"` specify kiya jata hai, jo global client ke saath kaam karta hai.
- `Runner.run_sync` ke zariye agent query ("Hello") ke saath chalta hai aur global settings use karta hai.
**File**: `global_level.py`

## Priority of Levels
Repository kehti hai: **"We will always use your Agent Level Configuration so each agent can use the LLM best fit for it."** Iska matlab hai:
- **Agent Level** sabse zyada priority rakhta hai. Agar aapne agent ke liye model aur provider set kiya, toh wohi use hoga, chahe Run Level ya Global Level pe kuch aur set ho.
- **Run Level** doosri priority hai. Agar agent ke liye model set nahi hai, toh RunConfig ke settings use honge.
- **Global Level** sabse neeche hai. Agar na Agent Level na Run Level pe kuch specify kiya, toh global settings apply hongi.

**Example**:
- Agar Agent Level pe Gemini set kiya, toh wohi use hoga, chahe Global Level pe OpenAI set ho.
- Agar Agent Level pe model nahi diya, lekin Run Level pe `RunConfig` mein model set hai, toh Run Level wala use hoga.
- Agar dono jagah kuch nahi set, toh Global Level ka default provider aur model use hoga.

## Kaise Run Karen
1. Repository clone karen:
   ```bash
   git clone https://github.com/panaversity/learn-agentic-ai.git
   ```
2. Folder mein jayein:
   ```bash
   cd learn-agentic-ai/01_ai_agents_first/05_model_configuration
   ```
3. Apna API key code mein daalen (e.g., `gemini_api_key = "your_api_key_here"`).
4. Dependencies install karen:
   ```bash
   pip install -r ../../requirements.txt
   ```
5. Koi bhi script run karen:
   ```bash
   python agent_level.py
   ```
   Ya
   ```bash
   python run_level.py
   ```
   Ya
   ```bash
   python global_level.py
   ```

## Ahem Notes
- API key ko securely store karen aur public na karen.
- Google Gemini ke API rate limits check karen (masalan, 1,500 requests/day).
- Tracing disable karne se performance behtar ho sakti hai, lekin debugging ke liye enable rakhna behtar hai.
- Aur examples ke liye repository ke doosre folders dekhen, jaise `02_openrouter` ya `10_context`.

## Resources
- [OpenAI Agents SDK Documentation](https://openai.github.io/openai-agents-python/)
- [Google Gemini API](https://ai.google.dev/gemini-api/docs/openai)
- [OpenRouter API](https://openrouter.ai/docs)

## License
Yeh repository **MIT License** ke tehat hai. Code ko freely use, modify, aur share kar sakte hain, lekin original copyright notice rakhna zaroori hai.