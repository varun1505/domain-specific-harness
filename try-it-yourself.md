# Try it yourself

The Jyotish tools from the talk are live on a public MCP server. **Free. No sign-up.** Plug them into any agent or harness that speaks MCP, then ask a question in plain words.

**Server:** `https://mcp.jyotisha.fyi/mcp` (MCP over HTTP)

## 1. Connect your agent

**Claude Code**

```
claude mcp add --transport http jyotish https://mcp.jyotisha.fyi/mcp
```

**Claude (web or desktop app)**

Settings → Connectors → Add custom connector → paste `https://mcp.jyotisha.fyi/mcp`

**Cursor**

Add this to `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global):

```json
{
  "mcpServers": {
    "jyotish": { "url": "https://mcp.jyotisha.fyi/mcp" }
  }
}
```

**Any other agent or harness**

Point its MCP client at `https://mcp.jyotisha.fyi/mcp`. If you are building your own harness, list the tools with `tools/list` and call them like any other tool.

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
