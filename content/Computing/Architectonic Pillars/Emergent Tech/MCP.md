# Model Context Protocol (MCP)

MCP is an open standard (introduced by Anthropic, 2024) that defines how LLM-powered applications connect to external tools, data sources, and services through a uniform interface — analogous to what USB-C is to hardware peripherals.

---

## Core Model

```
Host (LLM app)
  └── MCP Client  ──────────►  MCP Server  ──────────►  Resource / Tool
         (protocol layer)        (adapter)               (DB, API, file system)
```

Three roles:

| Role | Responsibility |
|------|---------------|
| **Host** | The AI application (Cursor, Claude Desktop, custom agent). Initiates connections. |
| **MCP Client** | Lives inside the host; manages one-to-one connections with servers. |
| **MCP Server** | Lightweight adapter that exposes capabilities (tools, resources, prompts) over the protocol. |

---

## What a Server Can Expose

### Tools
Executable functions the LLM can call with arguments. The server declares a JSON Schema for each tool; the host decides whether to invoke it.

```json
{
  "name": "query_database",
  "description": "Run a read-only SQL query",
  "inputSchema": {
    "type": "object",
    "properties": { "sql": { "type": "string" } },
    "required": ["sql"]
  }
}
```

### Resources
Read-only data sources (files, DB rows, API responses) identified by URI. The LLM can request them for context.

```
resource URI:  file:///project/src/main.java
resource URI:  db://customers/id/42
```

### Prompts
Reusable prompt templates with parameters, surfaced to the user as slash commands or slash prompts.

---

## Transport Layer

MCP supports two transports:
- **stdio** — local servers (subprocess); host writes JSON-RPC to stdin, reads from stdout.
- **HTTP + SSE** — remote servers; server-sent events for streaming, HTTP POST for requests.

---

## Lifecycle

1. Host starts and discovers available MCP servers (config or discovery endpoint).
2. **Initialise** handshake: client sends `initialize`, server replies with capabilities.
3. Host exposes tool/resource list to the LLM.
4. LLM decides to call a tool → host routes call to correct server.
5. Server executes, returns result → host injects into LLM context.
6. **Shutdown** message when session ends.

---

## Why It Matters (Emergent Tech angle)

Before MCP, every AI app hand-rolled its own function-calling glue. MCP standardises:
- **Context injection** — give the LLM only the data it needs, not the whole DB.
- **Security boundary** — servers control what is exposed; LLMs cannot call arbitrary code.
- **Composability** — swap tools without changing the host; mix servers from multiple vendors.

Cross-pillar links:
- `[[Computing/Architectonic Pillars/System/Infrastructure/Message Queue]]` — MCP over SSE shares pub/sub patterns
- `[[Computing/Architectonic Pillars/System/API Design]]` — MCP uses JSON-RPC 2.0 over HTTP; same API design concerns apply
- `[[Computing/Architectonic Pillars/Emergent Tech/Agent Transition]]` — MCP is a key enabler of agentic pipelines

---

## Resources

- [MCP Specification](https://spec.modelcontextprotocol.io)
- [Anthropic MCP Introduction](https://www.anthropic.com/news/model-context-protocol)
- [[Vishrut Inputs]] — tool inventory including MCP-compatible servers
