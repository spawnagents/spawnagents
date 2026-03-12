# spawnagents

**Spawn and control AI agents programmatically.**

> Full release coming soon — [spawnagents.dev](https://spawnagents.dev)

---

## What is SpawnAgents?

SpawnAgents is a platform and SDK for creating, deploying, and orchestrating AI agents without managing infrastructure. You define what the agent should do; SpawnAgents handles the runtime, memory, tool execution, and lifecycle.

Each agent is a persistent, stateful process — not a one-shot LLM call. Agents can browse the web, write and run code, call APIs, send messages, and coordinate with other agents. You interact with them over a simple API or stream their output in real time.

---

## Core concepts

### Spawn an agent

```ts
import { spawnagents } from 'spawnagents'

const agent = await spawnagents.spawn({
  name: 'researcher',
  model: 'claude-opus-4-6',
  instructions: 'You are a research assistant. When given a topic, find the latest information and summarise it.',
})
```

### Run a task

```ts
const result = await agent.run('Summarise the latest news on open-source LLMs')

console.log(result.output)
// → "Here is a summary of the latest developments in open-source LLMs..."
```

### Stream output in real time

```ts
const stream = agent.stream('Write a full business plan for a SaaS analytics tool')

for await (const chunk of stream) {
  process.stdout.write(chunk.text)
}
```

### Give agents tools

```ts
const agent = await spawnagents.spawn({
  name: 'dev-agent',
  model: 'claude-opus-4-6',
  instructions: 'You are a senior software engineer.',
  tools: ['code_execution', 'web_search', 'file_system'],
})
```

### Persistent memory

Agents remember context across sessions. Each agent has its own isolated workspace that persists between calls — files, state, conversation history.

```ts
// First session
await agent.run('Read the file data.csv and build a summary')

// Later — same agent, same context
await agent.run('Now generate a chart from that data')
```

### Multi-agent orchestration

```ts
const planner = await spawnagents.spawn({ name: 'planner', ... })
const executor = await spawnagents.spawn({ name: 'executor', ... })

const plan = await planner.run('Break down this project into tasks: ...')
const result = await executor.run(plan.output)
```

### MCP support

SpawnAgents agents are compatible with the [Model Context Protocol](https://modelcontextprotocol.io). Connect any MCP server to extend what your agents can do.

```ts
const agent = await spawnagents.spawn({
  name: 'power-agent',
  mcp: ['https://mcp.yourservice.dev/sse'],
})
```

---

## REST API

Every agent is also accessible over HTTP with a stable endpoint — useful for webhooks, Zapier, n8n, or any no-code platform.

```http
POST https://api.spawnagents.dev/v1/agents/{id}/run
Authorization: Bearer sk-sa-...
Content-Type: application/json

{ "input": "Analyse this CSV and return insights" }
```

---

## Designed for developers and teams

- **Instant setup** — no Docker, no VMs, no GPU provisioning
- **Bring your own model** — Claude, GPT-4o, Gemini, open-source models
- **Team access** — invite teammates, share agents, manage permissions
- **Audit logs** — full trace of every agent action
- **SDK + REST** — Node.js SDK with full TypeScript support; REST API for everything else

---

## Roadmap

- [ ] Node.js SDK (`spawnagents`)
- [ ] REST API + streaming
- [ ] Persistent agent workspaces
- [ ] Built-in tools (web, code, files, email, calendar)
- [ ] MCP server compatibility
- [ ] Multi-agent orchestration
- [ ] Python SDK
- [ ] Dashboard at [spawnagents.dev](https://spawnagents.dev)

---

## Stay updated

- Website: [spawnagents.dev](https://spawnagents.dev)
- GitHub: [github.com/spawnagents](https://github.com/spawnagents)
- npm: [npmjs.com/package/spawnagents](https://www.npmjs.com/package/spawnagents)

---

## License

MIT
