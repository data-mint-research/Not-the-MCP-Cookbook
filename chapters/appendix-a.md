# Appendix A: MCP Glossary

This glossary provides definitions for key concepts and entities used throughout the MCP ecosystem. Terms are grouped by category.

---

## 🔹 Protocol Roles

### **Client**
An entity (usually an LLM or agent) that initiates requests to a Host. Clients are untrusted and cannot directly access Servers.

### **Host**
The trusted mediator between Clients and Servers. It routes calls, enforces schemas, scopes roots, and may implement user consent or rate-limiting logic.

### **Server**
The responder to Host-initiated requests. Implements logic, tools, and context operations such as `tool/call`, `resource/read`, or `sampling/createMessage`.

---

## 🔹 Endpoints

### **tool/list**
Returns available tool definitions, schemas, and metadata.

### **tool/call**
Executes a tool based on given parameters.

### **resource/list**
Returns list of available context resources (files, docs, APIs).

### **resource/read**
Fetches the content or metadata of a specific resource.

### **prompt/render**
Renders a reusable template prompt with injected input values.

### **sampling/createMessage**
Delegates a model call to an LLM backend via a Server.

---

## 🔹 Concepts

### **Root**
A scoped base context (e.g. `file:///project`, `memory://user-session`) for resolving relative resource URIs and enforcing sandboxing.

### **Tool**
A JSON-defined function with a schema for parameters and optional confirmation/destructive flags. Executed via a Server.

### **Prompt**
A reusable, structured system message or template intended for LLMs.

### **Sampling**
Controlled generation of output messages from an LLM, initiated via a Server — never directly from a Client.

### **Confirmation**
Host-enforced requirement that a tool or action be explicitly confirmed before being executed. May involve user interaction.

---

## 🔹 Technical Terms

### **Schema**
A JSON-based description of the parameters accepted by a tool. Used by Hosts to validate inputs.

### **Intent Confirmation**
A flow where the Client must reconfirm a proposed tool use before execution proceeds.

### **Zero Trust**
A security model where no component is inherently trusted — all actions must be explicitly allowed and validated.

### **Idempotent**
A tool or endpoint that produces the same result when called multiple times with the same inputs.

### **Composability**
The ability to combine multiple tools, resources, and prompts into larger workflows while preserving security boundaries.

---

This glossary will be extended with examples, code links, and visual schemas in future versions.
