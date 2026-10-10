# Try it yourself

The Jyotish tools from the talk are live on a public MCP server. **Free. No sign-up.** Plug them into any agent or harness that speaks MCP, then ask a question in plain words.

**Server:** `https://mcp.jyotisha.fyi/mcp` (MCP over HTTP)

## 1. Connect your agent

Pick your tool. Every option points at the same address: `https://mcp.jyotisha.fyi/mcp`.

| Tool | How |
|---|---|
| [Claude Code](#claude-code) | one command |
| [Claude Desktop / claude.ai](#claude-desktop-and-claudeai) | Connectors menu, or a config file |
| [Cursor](#cursor) | `mcp.json` |
| [VS Code (Copilot agent mode)](#vs-code) | one command, or `mcp.json` |
| [Codex CLI](#codex-cli) | one command |
| [Gemini CLI](#gemini-cli) | one command |
| [OpenCode](#opencode) | `opencode.json` |
| [Windsurf](#windsurf) | `mcp_config.json` |
| [Anything else](#anything-else) | URL, or a local bridge |

### Claude Code

```
claude mcp add --transport http jyotish https://mcp.jyotisha.fyi/mcp
```

Add `--scope user` to use it in every project, not just the current one.

### Claude Desktop and claude.ai

**Option A: the Connectors menu** (simplest)

1. Open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Name it `jyotish` and paste `https://mcp.jyotisha.fyi/mcp`.
4. In a new chat, turn the connector on from the tools menu.

A connector added on claude.ai also shows up in the desktop app. If you don't see **Add custom connector** on your plan, use Option B.

**Option B: the config file** (desktop app, any plan)

Open **Settings → Developer → Edit Config**. That opens `claude_desktop_config.json`:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Add this, then quit and reopen Claude:

```json
{
  "mcpServers": {
    "jyotish": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.jyotisha.fyi/mcp"]
    }
  }
}
```

This runs [`mcp-remote`](https://www.npmjs.com/package/mcp-remote), a small local bridge to the server. It needs Node.js installed. If the file already has an `mcpServers` block, add `jyotish` inside it instead of adding a second block.

### Cursor

**Settings → MCP → Add new MCP server**, or add this to `.cursor/mcp.json` (one project) or `~/.cursor/mcp.json` (every project):

```json
{
  "mcpServers": {
    "jyotish": { "url": "https://mcp.jyotisha.fyi/mcp" }
  }
}
```

### VS Code

For GitHub Copilot in agent mode. One command:

```
code --add-mcp '{"name":"jyotish","type":"http","url":"https://mcp.jyotisha.fyi/mcp"}'
```

Or add this to `.vscode/mcp.json` in your project:

```json
{
  "servers": {
    "jyotish": { "type": "http", "url": "https://mcp.jyotisha.fyi/mcp" }
  }
}
```

### Codex CLI

```
codex mcp add jyotish --url https://mcp.jyotisha.fyi/mcp
```

### Gemini CLI

```
gemini mcp add --transport http --scope user jyotish https://mcp.jyotisha.fyi/mcp
```

### OpenCode

Add this to `opencode.json` in your project, or to `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "jyotish": { "type": "remote", "url": "https://mcp.jyotisha.fyi/mcp", "enabled": true }
  }
}
```

### Windsurf

Add this to `~/.codeium/windsurf/mcp_config.json`, then refresh the MCP list in Cascade:

```json
{
  "mcpServers": {
    "jyotish": { "serverUrl": "https://mcp.jyotisha.fyi/mcp" }
  }
}
```

### Anything else

- **Your tool supports remote MCP servers:** give it `https://mcp.jyotisha.fyi/mcp` as an HTTP (streamable HTTP) server.
- **Your tool only runs local servers:** use the bridge as the command: `npx -y mcp-remote https://mcp.jyotisha.fyi/mcp`
- **You are building your own harness:** call `tools/list` and use the tools like any other. No API key needed.
- **Just want to poke at it:** run `npx @modelcontextprotocol/inspector`, choose *Streamable HTTP*, and connect to the URL.

## 2. Ask it something

Give three things: your **birth date**, your **birth time** (as the clock showed where you were born), and your **birth city with the country**. Then ask a real question.

> I was born on 14 March 1992 at 6:45 am in Nagpur, India. When will I get married?

> I was born on 21 June 1988 at 11:20 pm in Mumbai, India. Is next year good for a job change?

> I was born on 5 January 2000 at 4:10 pm in Bengaluru, India. Am I in sade sati right now?

> I was born on 2 October 1994 at 9:30 pm in Pune, India. When will I become a millionaire?

> I was born on 8 August 1985 at 7:05 am in Chennai, India. What does my chart say about my career?

> I was born on 30 November 1996 at 2:15 pm in Delhi, India. Which years look best for buying a house?

> My daughter was born on 12 February 2019 at 10:30 am in Hyderabad, India. What is she likely to be good at?

**Try the same question on two different models.** The tools are the same, so any difference you see comes from the model. That is the idea behind [JyotishBench](https://jyotishbench.fyi).

## What the tools do

There are ten. Your agent calls `cast_chart` first. It returns a birth block that every other tool takes back as-is.

| Tool | What it answers |
|---|---|
| `cast_chart` | The full birth chart. Always first. |
| `house_report` | Everything about one area of life (a "house"): who rules it, what sits in it, what looks at it |
| `graha_report` | Everything about one planet: where it is and how strong it is |
| `moment_report` | What is running for you on a given date |
| `dasha_at` | Which life period you are in on a date |
| `dasha_timeline` | All the life periods between two dates |
| `find_transit` | When a planet reaches a spot in your chart |
| `find_windows` | When the right life period and the right planet move line up |
| `saturn_report` | Sade sati, with dates for each phase |
| `varga` | A zoomed-in divisional chart, such as D9 or D10 |

## Why the answers stay grounded

- **Code does the maths.** Every planet position, period date and transit is computed with Swiss Ephemeris. The model never estimates one.
- **The timezone comes from the place and the date,** not from you. Kolkata in 1945 was +06:30, not +05:30.
- **The rules travel with the tools.** `cast_chart` hands your agent the grounding rules too: use only these facts, never guess a date, no medical diagnosis, no guarantees, nothing about death.

## Tips

- **Birth time matters most.** Four minutes moves the rising sign by about one degree. If your time is a guess, treat the reading as a guess.
- Add the country to the city. "Pune, India", not just "Pune".
- If the place comes back wrong, give latitude and longitude instead.

## A note

This is a demo of harness engineering, not a claim that astrology works. Nothing here is medical, legal or financial advice.
