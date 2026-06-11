# AgentGauge skill for Claude Code

Scan any website's **AI readiness** from inside your AI coding tool — then let it fix
the findings in your codebase and re-scan to verify.

[AgentGauge](https://www.agentgauge.ai) grades whether **AI search can find you** (citability,
llms.txt, schema, AI-crawler access) and whether **AI agents can use you** (tools, identity,
commerce signals), against the [AgentReady standard](https://www.agentready.org/) with stable
AR-IDs and MUST/SHOULD/MAY conformance. Free scan, instant letter grade.

## Install

### 1. The MCP server (recommended)

```bash
claude mcp add --transport http agentgauge https://mcp.agentgauge.ai/mcp
```

Cursor / Claude Desktop JSON config:

```json
{
  "mcpServers": {
    "agentgauge": { "url": "https://mcp.agentgauge.ai/mcp" }
  }
}
```

No auth. One tool: `scan_website({ url })`.

### 2. The skill

Copy `SKILL.md` into your Claude Code skills directory as `agentgauge/SKILL.md`:

```bash
mkdir -p ~/.claude/skills/agentgauge
curl -fsSL https://raw.githubusercontent.com/GlassCaseOfEmotion/agentgauge-skill/main/SKILL.md \
  -o ~/.claude/skills/agentgauge/SKILL.md
```

## Use

```
/agentgauge scan mysite.com
```

or just ask: *"check my site's AI readiness and fix what's failing"*. The skill scans,
prioritizes the findings (spec MUSTs first), implements fixes in your repo, and re-scans
after deploy to verify the grade actually moved.

## What gets checked

31 checks across 6 categories: Discoverability, Content for Agents, AI Visibility (GEO),
Capabilities (MCP/A2A/OpenAPI), Identity & Access, and Commerce.

---

Built by [AgentGauge](https://www.agentgauge.ai) · Weekly monitoring:
[Watch by AgentGauge](https://www.agentgauge.ai/watch) · Contact: support@agentgauge.ai
