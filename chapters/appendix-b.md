# Appendix B: MCP Tool Schema Reference

This appendix defines the standard structure of a **MCP-compliant tool**, including its metadata, parameter schema, and execution requirements. Tools are declared via `tool/list` and invoked via `tool/call`.

---

## 🔹 Tool Metadata
Each tool must be represented as a JSON object with the following fields:

### Required Fields
| Field         | Type     | Description                                               |
|---------------|----------|-----------------------------------------------------------|
| `id`          | string   | Unique, URL-safe identifier for the tool                 |
| `label`       | string   | Human-readable name                                       |
| `description` | string   | Brief explanation of what the tool does                 |
| `parameters`  | object   | JSON Schema describing input arguments                   |

### Optional Fields
| Field            | Type    | Description                                                                 |
|------------------|---------|-----------------------------------------------------------------------------|
| `confirmation`   | boolean | If true, Host must confirm intent before execution                         |
| `destructive`    | boolean | If true, the tool may alter state or external systems                      |
| `category`       | string  | Optional UI group (e.g., "filesystem", "git")                             |
| `examples`       | array   | Example inputs for UI preview or validation                               |
| `tags`           | array   | List of tag strings for search/classification                             |

---

## 🔹 Example Tool Definition
```json
{
  "id": "write-file",
  "label": "Write File",
  "description": "Writes content to a specified file path.",
  "parameters": {
    "type": "object",
    "required": ["path", "contents"],
    "properties": {
      "path": {
        "type": "string",
        "description": "Absolute or relative file path"
      },
      "contents": {
        "type": "string",
        "description": "Text content to write"
      }
    }
  },
  "confirmation": true,
  "destructive": true,
  "category": "filesystem"
}
```

---

## 🔹 Parameter Schema Design
Tool parameters must follow valid **JSON Schema Draft 7+**. Best practices include:

- Use `type`, `description`, and `required` consistently
- Prefer `enum` for closed-choice options
- Avoid nesting unless absolutely necessary

### Common Field Types
| Type       | JSON Schema Syntax              |
|------------|----------------------------------|
| String     | `{ "type": "string" }`          |
| Number     | `{ "type": "number" }`          |
| Boolean    | `{ "type": "boolean" }`         |
| Object     | `{ "type": "object", ... }`      |
| Array      | `{ "type": "array", ... }`       |
| Enum       | `{ "enum": ["A", "B"] }`         |

---

## 🔹 Tool Execution Semantics
- Inputs are always passed as a single JSON object to `tool/call`
- Output must be a single JSON-serializable object
- On failure, return an error object with `error.message` and optional `error.code`

### Example Success Response
```json
{
  "result": "File written successfully."
}
```

### Example Error Response
```json
{
  "error": {
    "message": "Permission denied",
    "code": "E_ACCESS"
  }
}
```

---

This schema enables secure, explainable tool use across MCP Hosts and Clients. For advanced patterns (tool chaining, delegation), see later appendices.

