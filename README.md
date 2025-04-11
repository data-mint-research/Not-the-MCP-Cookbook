# Not-the-MCP-Cookbook

**A comprehensive guide to building secure, modular, and explainable agent systems using the Model Context Protocol (MCP).**

> This is not just documentation. It’s an operational manual, architecture reference, and training blueprint for orchestrated LLM systems.

---

## 📘 What You'll Learn
- What MCP is and why it matters
- How to write MCP-compliant servers, hosts, and clients
- Secure LLM usage with root-scoped sandboxing
- Declarative tools, reusable prompts, and memory roots
- Multi-agent orchestration with LangGraph, Autogen, and more
- CI/CD-ready deployment recipes (local, Docker, GitHub)
- Advanced diagnostics, adapters, registries, and patterns

---

## 📚 Table of Contents
```
* [Introduction](README.md)
* [1. MCP as Architecture](part1/mcp-as-architecture.md)
* [2. Writing an MCP Server](part2/writing-a-server.md)
* [3. Using Tools and Prompts](part2/tools-prompts.md)
* [4. Sampling and Roots](part2/sampling-roots.md)
* [5. Security & Best Practices](part3/security.md)
* [6. Deployment Recipes](part4/recipes.md)
* [Developer Onboarding](onboarding_dev.md)
* [Appendix A: Glossary](appendix_glossary.md)
* [Appendix B: Tool Schema Reference](appendix_tool_schema.md)
* [Appendix C: Endpoint Specification](appendix_endpoint_spec.md)
* [Appendix D: Security Blueprint](appendix_security_blueprint.md)
* [Appendix E: Starter Templates](appendix_templates.md)
* [Appendix F: Use Case Catalog](appendix_use_cases.md)
* [Appendix G: Registry & Discovery](appendix_registry_discovery.md)
* [Appendix H: Workflow Orchestration](appendix_workflow_orchestration.md)
* [Appendix I: LLM Adapters](appendix_llm_adapters.md)
* [Appendix J: Diagnostic Tools](appendix_diagnostics.md)
```

---

## 🗺 Visual MCP Overview

```mermaid
graph TD
  subgraph Client
    A1[LLM or User]
  end
  subgraph Host
    A2[MCP Host: Routes & Confirms]
  end
  subgraph Server
    S1[tool/call], S2[resource/read], S3[sampling/createMessage]
  end

  A1 --> A2 --> S1 --> A2 --> A1
  A2 --> S2
  A2 --> S3
```

---

## 📦 Quick Start
```bash
# Install HonKit
npm install -g honkit

# Serve locally
honkit serve

# Build static site
honkit build
```

---

## 📡 Live Deployment
This project is configured to auto-deploy via GitHub Actions to GitHub Pages.

> Push to `main` → HonKit builds → GitHub Pages updates.

Enable Pages under your repo settings:  
`Settings > Pages > Source: GitHub Actions`

---

## ✨ Powered by
- Claude 3 (structure + content)
- DeepSeek Code (examples + syntax)
- Mermaid (diagrams)
- HonKit (rendering engine)

---

## 🧠 Maintainers
This project is maintained by the SEMA project team and aligned with the MCP reference implementation roadmap.

Want to extend it? Fork and PR your own appendices, tools, or recipes.

---

> _“Structure is the scaffolding of safe autonomy.”_

