# Grafizum Command Center

**Grafizum Command Center is the dashboard for developers coordinating AI coding agents across projects.**

One dashboard for projects, tasks, agent status, blockers, project memory and work waiting for review. Claude Code, Cursor, Codex and any other MCP client connect over remote MCP with OAuth and keep the board up to date themselves.

- Website: https://grafizum.com
- Dashboard: https://dashboard.grafizum.com
- MCP endpoint: `https://mcp.grafizum.com/mcp` (Streamable HTTP, OAuth 2.1)
- Free plan: 2 active projects, 500 automated updates a month, no card

This repository holds the setup guide and example configs. Grafizum itself is a hosted service.

## The problem

Run more than two or three AI coding agents and you lose track: which session is still working, which one is blocked, what the agent in the other project changed yesterday. The answers are spread over terminal tabs and chat windows.

Grafizum puts them on one screen. Each agent takes a task, marks it in progress, records what it changed and hands finished work to a review queue.

## Quick start

1. Create a free account at https://grafizum.com/signup and open a project.
2. Add the server to your client (below).
3. Approve the connection once in the dashboard and pick the projects it may use.
4. Ask your agent: "What is on the Grafizum board for this project?"

There is no API key to copy. The client signs in with OAuth in the browser and renews its own token.

### Claude Code

```bash
claude mcp add --transport http grafizum https://mcp.grafizum.com/mcp
```

### Cursor, GitHub Copilot and other clients that read `.mcp.json`

```json
{
  "mcpServers": {
    "grafizum": { "type": "http", "url": "https://mcp.grafizum.com/mcp" }
  }
}
```

See [`examples/mcp.json`](examples/mcp.json).

### VS Code

Same two fields under a top-level `servers` object in `.vscode/mcp.json`. See [`examples/vscode-mcp.json`](examples/vscode-mcp.json).

### Any other MCP client

Codex, Gemini CLI, Windsurf, Cline, Zed, Raycast, Goose, opencode, LM Studio: add `https://mcp.grafizum.com/mcp` as a remote (HTTP) MCP server in the client's own settings and let it run the OAuth flow.

Discovery documents, for clients that register themselves:

- `https://mcp.grafizum.com/.well-known/oauth-authorization-server`
- `https://mcp.grafizum.com/.well-known/oauth-protected-resource/mcp`

## What a connected agent can do

| Read | Write | Never |
|---|---|---|
| Projects, status and progress | Create and update tasks | Vault secrets, GitHub tokens or SSH keys |
| Tasks and queues | Report project status | Projects the connection was not granted |
| Calendar and due dates | Record events and calendar entries | Your review decisions |
| Project memory | Write project memory (within its scopes) | |

The exact tool list is on https://grafizum.com/mcp.

## Two sync modes

- **Auto** (default): the agent keeps the board current as verified work finishes.
- **Manual**: the agent reads freely but only writes when you ask.

Switch by telling the agent "switch Grafizum to manual" or "back to auto".

## Security

- A connection reaches only the projects and scopes you granted. Revoke it any time.
- Every write lands on an append-only record with the name of the agent that made it.
- Keys are stored as a hash and shown once.

## Pricing

| Plan | Price | Projects | Automated updates / month |
|---|---|---|---|
| Free | $0 | 2 | 500 |
| Starter | $9 / month | 20 | 10,000 |
| Team | $24 / month, 4 people | 50 | 40,000 |
| Business | $99 / month, 20 people | 200 | 200,000 |

Reads are never counted. No fixed limit on connected clients. Details: https://grafizum.com/pricing

## What Grafizum is not

- Not an agent runner: it does not start agents, manage git worktrees or show terminal output.
- Not a model provider: your own agent does the work.

## More

- [What is Grafizum?](https://grafizum.com/what-is-grafizum)
- [AI coding agent dashboard: how to manage multiple AI agents](https://grafizum.com/ai-coding-agent-dashboard)
- [FAQ](https://grafizum.com/faq)
- Questions: info@grafizum.com, or open an issue here.
