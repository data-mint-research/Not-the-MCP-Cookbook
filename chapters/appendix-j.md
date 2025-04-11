# Appendix J: MCP Diagnostic & Testing Tools

This appendix presents tools, scripts, and techniques to test and validate MCP Hosts, Servers, and Clients.

These tools help developers:
- Validate protocol compliance
- Test schema correctness
- Monitor server health
- Debug tool or sampling errors

---

## ✅ Tool 1: `mcp-ping`

### Purpose
Check if a server responds to required endpoints (`tool/list`, `tool/call`, `resource/list`, etc.)

### Example
```bash
http GET http://localhost:5000/tool/list
```

### What to look for
- HTTP 200 response
- Valid JSON
- List of tools with `id`, `label`, `parameters`

---

## 🧪 Tool 2: `validate_tool_schema.py`

### Purpose
Ensure all tools returned from `tool/list` use valid JSON Schema

### Example
```python
import jsonschema, requests

resp = requests.post("http://localhost:5000/tool/list")
for tool in resp.json():
    jsonschema.Draft7Validator.check_schema(tool["parameters"])
```

---

## 📊 Tool 3: `mcp-healthcheck`

### Purpose
Run a full liveness + functionality test on one or more MCP Servers

### Sample Workflow
```bash
curl http://localhost:5000/tool/list
curl -X POST http://localhost:5000/tool/call \
     -H "Content-Type: application/json" \
     -d '{"text": "ping"}'
```

---

## 🧠 Tool 4: `simulate_llm_request.py`

### Purpose
Send a simulated `sampling/createMessage` request to an adapter or server

### Example
```python
import requests

payload = {
  "input": {"role": "user", "content": "Summarize this text"},
  "history": [],
  "config": {"temperature": 0.7, "max_tokens": 200}
}
res = requests.post("http://localhost:11434/api/chat", json=payload)
print(res.json())
```

---

## 🧾 Tool 5: MCP JSON Contract Snapshots

Use a version-controlled folder with test payloads and expected outputs to:
- Run regression tests
- Check behavior across MCP versions

### Structure
```
tests/contracts/
├── tool_call_echo.json
├── resource_read_test.json
├── sampling_request_basic.json
```

---

## 🧱 Tool 6: MCP Validator (planned)
A future CLI tool that will:
- Run full compliance suite
- Validate endpoint behavior and response types
- Report missing or malformed fields
- Suggest fixes and health scores

> Proposed command: `mcp validate --url http://localhost:5000`

---

With these diagnostics, you can continuously verify that your MCP components are functional, interoperable, and secure.

