# Appendix C: MCP Endpoint Specification

This appendix defines the standard input/output schemas for each supported MCP endpoint. These are structured for HTTP-compatible JSON-RPC over REST.

---

## 🔹 `/resource/list`

**Method:** `GET`

### Returns:
```json
[
  {
    "id": "file-1",
    "label": "main.py",
    "type": "file",
    "uri": "file:///project/main.py"
  },
  ...
]
```

---

## 🔹 `/resource/read`

**Method:** `GET`
**Query Params:** `id` (string)

### Returns:
```json
{
  "id": "file-1",
  "uri": "file:///project/main.py",
  "contents": "print(\"Hello MCP\")",
  "mime": "text/x-python"
}
```

---

## 🔹 `/tool/list`

**Method:** `POST`

### Returns:
```json
[
  {
    "id": "write-file",
    "label": "Write File",
    "description": "Writes to a file",
    "parameters": { ... },
    "confirmation": true
  },
  ...
]
```

---

## 🔹 `/tool/call`

**Method:** `POST`
**Body:**
```json
{
  "id": "write-file",
  "parameters": {
    "path": "test.txt",
    "contents": "Hello"
  }
}
```

### On Success:
```json
{
  "result": "OK"
}
```

### On Error:
```json
{
  "error": {
    "message": "File not found",
    "code": "E_NOT_FOUND"
  }
}
```

---

## 🔹 `/prompt/list`

**Method:** `GET`

### Returns:
```json
[
  {
    "id": "summarize-json",
    "template": "Summarize this JSON: {{json}}",
    "inputs": ["json"]
  }
]
```

---

## 🔹 `/prompt/render`

**Method:** `POST`
**Body:**
```json
{
  "id": "summarize-json",
  "values": {
    "json": "{\"a\":1}"
  }
}
```

### Returns:
```json
{
  "text": "Summary: JSON with a=1"
}
```

---

## 🔹 `/sampling/createMessage`

**Method:** `POST`
**Body:**
```json
{
  "input": {
    "role": "user",
    "content": "Summarize this"
  },
  "history": [],
  "config": {
    "temperature": 0.5,
    "max_tokens": 256
  },
  "tools": [ ... ],
  "root": "file:///project/"
}
```

### Returns:
```json
{
  "message": {
    "role": "assistant",
    "content": "This is the summary..."
  }
}
```

---

This endpoint reference acts as a compatibility contract. Hosts and Servers must strictly validate types, structure, and intent to ensure safety and interoperability.

