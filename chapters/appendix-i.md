Appendix I: MCP-Compatible LLM Adapters

This appendix outlines how to connect various language model providers (local and cloud) to the MCP sampling/createMessage endpoint.

Each adapter is responsible for:
	•	Accepting MCP-compatible payloads
	•	Calling the target LLM with appropriate formatting
	•	Returning a valid message response

⸻

🌩 OpenAI Adapter (Chat Completions API)

Requirements
	•	openai Python SDK
	•	OpenAI API key via environment variable

Code Snippet

import openai

def mcp_sampling_openai(payload):
    history = payload.get("history", [])
    input_msg = payload["input"]
    messages = history + [input_msg]
    
    result = openai.ChatCompletion.create(
        model="gpt-4",
        messages=messages,
        temperature=payload.get("config", {}).get("temperature", 0.7),
        max_tokens=payload.get("config", {}).get("max_tokens", 512)
    )
    return {"message": result["choices"][0]["message"]}



⸻

🤖 Claude Adapter (Anthropic Messages API)

Requirements
	•	anthropic Python SDK
	•	API key via ANTHROPIC_API_KEY

Code Snippet

import anthropic

def mcp_sampling_claude(payload):
    client = anthropic.Anthropic()
    messages = payload.get("history", []) + [payload["input"]]

    result = client.messages.create(
        model="claude-3-opus-20240229",
        messages=messages,
        temperature=payload.get("config", {}).get("temperature", 0.7),
        max_tokens=512
    )
    return {"message": result.content[0].to_dict()}



⸻

🧠 Local Adapter: OpenRouter / LM Studio / Ollama

Requirements
	•	Local HTTP endpoint (e.g. http://localhost:11434/api/chat)

Generic Adapter (Ollama-style)

import requests

def mcp_sampling_local(payload):
    input_msg = payload["input"]
    body = {
        "model": "deepseek-coder:33b",
        "messages": payload.get("history", []) + [input_msg],
        "stream": False
    }
    res = requests.post("http://localhost:11434/api/chat", json=body)
    return {"message": res.json()["message"]}



⸻

Adapter Best Practices

Strategy	Description
Format compatibility	Ensure all message fields follow role/content
Token limits	Respect max_tokens and truncate history
Tool usage injection	Inject tool_choice or function_call info
Retry & timeout logic	Handle LLM latency and availability
Versioned metadata	Track model used and version in response



⸻

These adapters allow any MCP host or server to safely delegate LLM usage to a trusted backend — whether local, API, or hybrid.