---
name: mcp-server-patterns
description: >
  Build MCP servers with Node/TypeScript — tools, resources, prompts, Zod validation, stdio vs Streamable
  HTTP. Use when adding an MCP server, registering tools/resources, choosing transport, upgrading
  @modelcontextprotocol/sdk, debugging Cursor/Claude Desktop MCP connections, or exposing APIs to agents.
  Always verify current SDK signatures via official MCP docs or Context7 before copying patterns.
origin: ECC
---

# MCP Server Patterns

The Model Context Protocol (MCP) lets AI clients call **tools**, read **resources**, and use **prompts** from your server. SDK APIs change between releases — confirm method names against [modelcontextprotocol.io](https://modelcontextprotocol.io) or Context7 (`query-docs` for "MCP") before implementing.

## When to use

- New MCP server or new tool/resource on an existing server
- Choosing **stdio** vs **Streamable HTTP**
- SDK upgrade or broken registration after a version bump
- Cursor / Claude Desktop connection or auth issues

## Transport decision

```
Local desktop client (Claude Desktop, some CLI setups)?
  └─ Yes → stdio transport in the entrypoint

Remote client (Cursor cloud, shared service, browser gateway)?
  └─ Yes → Streamable HTTP (current spec)
       └─ Legacy clients only? → optional HTTP/SSE compat layer
```

Keep tool/resource logic **transport-agnostic**; wire stdio or HTTP only in `main` / server bootstrap.

## Core concepts

| Concept | Role |
| --- | --- |
| **Tools** | Actions the model invokes (search, run command, mutate state) |
| **Resources** | Read-only URIs (files, config snapshots, API payloads) |
| **Prompts** | Parameterized templates the client can surface |
| **Transport** | stdio (local) or Streamable HTTP (remote) |

Registration APIs differ by SDK version: `tool()` / `registerTool()`, `resource()` / `registerResource()`, object vs positional args. **Do not assume** the snippet below matches your pinned version — verify first.

## Minimal setup (verify against your SDK)

```bash
npm install @modelcontextprotocol/sdk zod
```

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { z } from "zod";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

// Register tools/resources using the API your SDK version documents.
// Example shape only — check docs for exact signature:
// server.tool({ name, description, inputSchema: z.object({...}) }, async (args) => ({ ... }))
```

Use **Zod** (or the SDK’s schema type) for every tool input. Describe return shape and failure modes in the tool **description** so the model chooses correctly.

## Security and ops (woodensword)

- **Secrets**: API keys and tokens only from environment or secret store — never in tool responses, resource bodies, or client-visible logs.
- **Auth**: Enforce auth on Streamable HTTP endpoints; do not rely on security through obscurity of tool names.
- **Input validation**: Treat all tool arguments as untrusted; validate and sanitize before side effects.
- **Errors**: Return structured, model-safe messages — no stack traces or internal paths to the client.
- **Cost / rate**: Document expensive or rate-limited tools in descriptions; implement backoff where needed.

## Best practices

- **Schema first** for every tool; document parameters and outputs.
- **Idempotency** where retries are likely.
- **Pin** `@modelcontextprotocol/sdk` in `package.json`; read release notes on upgrade.
- **Test** tools with the real client (Cursor MCP panel or Claude Desktop) — not only unit tests in isolation.

## Official SDKs

- **TypeScript**: `@modelcontextprotocol/sdk` (npm)
- **Go**: `modelcontextprotocol/go-sdk`
- **C#**: official .NET SDK

For registration and transport examples, prefer Context7 or the official docs over stale blog posts.

## Related skills

- **agent-builder** — when the MCP server is part of a broader agent harness
- **Fullstack** — when exposing MCP alongside a web app API
