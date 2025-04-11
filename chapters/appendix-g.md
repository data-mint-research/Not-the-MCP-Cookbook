# Appendix E: Starter Templates & Boilerplates

This appendix provides copy-paste-ready templates and boilerplate projects to kickstart MCP-compatible systems.

---

## 🐳 Docker Compose Setup
**Goal:** Run one Host and one Server in separate containers.

### `docker-compose.yml`
```yaml
version: '3.8'
services:
  mcp_server:
    build: ./server
    ports:
      - "5000:5000"
  mcp_host:
    build: ./host
    depends_on:
      - mcp_server
    ports:
      - "4000:4000"
```

**File structure:**
```
mcp-project/
├── docker-compose.yml
├── host/
│   └── Dockerfile
├── server/
│   └── Dockerfile
```

### Example `Dockerfile` (server/)
```Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install fastapi uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"]
```

---

## 🧱 GitHub Repo Boilerplate
Structure for a deployable GitHub project with HonKit and MCP logic:
```
not-the-mcp-cookbook/
├── .github/workflows/deploy.yml
├── README.md
├── SUMMARY.md
├── chapters/
│   └── *.md
├── server/
│   └── main.py
├── host/
│   └── host.py
```

**Deploy workflow:** See Appendix C `deploy.yml`

---

## ✏️ HonKit Init Script
To bootstrap a GitBook-compatible repo:
```bash
npm install -g honkit
honkit init
```
Creates:
```
./README.md
./SUMMARY.md
./book.json
```

---

## 📦 MCP Server Template (Python)
Basic server with `tool/list` and `tool/call`:
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Params(BaseModel):
    text: str

@app.post("/tool/list")
def list_tools():
    return [{
        "id": "echo",
        "label": "Echo",
        "description": "Echo input",
        "parameters": Params.schema()
    }]

@app.post("/tool/call")
def call_tool(input: Params):
    return {"result": f"Echo: {input.text}"}
```

---

## 🔁 GitHub Action MCP Linter (Optional)
Add this workflow to auto-check tool definitions:
```yaml
name: Validate MCP Tool Definitions
on: [push]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: pip install jsonschema
      - run: python3 scripts/validate_tools.py
```

---

These templates form a solid foundation for real-world MCP deployments. See [Developer Onboarding](../onboarding_dev.md) to build and extend your first project.

