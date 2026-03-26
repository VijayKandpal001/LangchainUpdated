# LangchainUpdated

## Overview

This repository contains LangChain tutorial notebooks demonstrating core concepts and model integrations, based on:
- `langchainContent/01_langchainIntro.ipynb`
- `langchainContent/02_modelIntegration.ipynb`

## Goals

- Provide hands-on examples for initializing LangChain LLM models
- Show agent creation with a custom tool using Groq
- Demonstrate integration with OpenAI, Google Gemini, and Groq model providers
- Show streaming and batch request patterns in LangChain

## Prerequisites

1. Python 3.8+
2. Virtual environment (recommended: `.venv`)
3. Install dependencies:
   - `pip install -r requirements.txt`
   - Required packages include:
     - `langchain`
     - `python-dotenv`
     - `langchain_google_genai`
     - `langchain_groq`
     - provider SDKs as needed (e.g., `openai`)
4. Create a `.env` file with:
   - `OPENAI_API_KEY`
   - `GOOGLE_API_KEY`
   - `GROQ_API_KEY`

## Notebook 1: langchainIntro

### Setup
- `import langchain`
- `from dotenv import load_dotenv`
- `load_dotenv()`
- read API keys from environment variables

### Agent + tool example
- Define a tool function:
  - `def getAQI(city: str) -> str`
  - returns `The AQI of {city} is 150.`
- Create agent:
  - `from langchain_groq import ChatGroq`
  - `from langchain.agents import create_agent`
  - model: `ChatGroq(model="llama-3.3-70b-versatile")`
  - tools: `[getAQI]`
  - system prompt: `"You are a helpful assistant..."`

### Sample invocation
`agent.invoke({"messages":[{"role":"user","content":"What is the AQI of the city of Dehradun?"}]})`

## Notebook 2: modelIntegration

### OpenAI model
- Use `langchain.chat_models.init_chat_model`
- Example: `model = init_chat_model("gpt-4.1")`
- `model.invoke("What is the capital of France?")`

### Google Gemini
- `from langchain_google_genai import ChatGoogleGenerativeAI`
- `llm = ChatGoogleGenerativeAI(model="gemini-1.5-pro")`
- Also available: `init_chat_model("google_genai:gemini-2.5-flash-lite")`
- Example: `modelgemini.invoke("What is the capital of Japan?")`

### Groq
- `modelgroq = init_chat_model("groq:qwen/qwen3-32b")`
- `modelgroq.invoke("What is the capital of Rajasthan?")`

### Streaming
- Streaming improves UX using `model.stream(...)`
- Example loop:
  - `for chunk in modelgemini.stream("Tell me about current affairs..."):`
  - `print(chunk.text, end="", flush=True)`

### Batch requests
- `modelgroq.batch([...], config={"max_concurrency": 3})`
- Optionally clean responses:
  - `re.sub(r'<think>.*?</think>', '', response.content, flags=re.DOTALL).strip()`

## Quick start code snippet

```python
from dotenv import load_dotenv
from langchain.chat_models import init_chat_model

load_dotenv()
model = init_chat_model("gpt-4.1")
print(model.invoke("What's the capital of France?").content)
```

## Notes

- API usage may incur costs and rate limits depending on provider.
- Ensure relevant API keys are set in `.env` before running notebooks.
