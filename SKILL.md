---
name: agentgauge
description: Scan a website's AI readiness with AgentGauge (can AI search find it? can AI agents use it?), then fix the findings in the codebase and re-scan to verify. Use when the user asks to check, improve, or fix a site's agent readiness, AI visibility, GEO, llms.txt, or AI-crawler setup.
---

# AgentGauge: scan and fix a website's AI readiness

AgentGauge (https://www.agentgauge.ai) grades how ready a website is for AI: whether AI
search engines can find and cite it (citability, llms.txt, schema, crawler access) and
whether AI agents can use it (tools, identity, commerce signals), graded against the
AgentReady standard with stable AR-IDs and MUST/SHOULD/MAY conformance.

## Step 1: Run the scan

Take the website URL from the user (or infer it from the project, e.g. the deployed
domain of the repo you're in — confirm it with the user first).

**Preferred — the AgentGauge MCP server** (if the `agentgauge` MCP server is connected,
use its `scan_website` tool):

```
scan_website({ url: "example.com" })
```

**Fallback — plain HTTP:**

```
POST https://www.agentgauge.ai/api/scan
Content-Type: application/json

{"url": "example.com"}
```

The response contains: `grade`, `overallScore`, `categories` (each check with status and
summary), `conformance` (MUST/SHOULD/MAY counts), and a `scanId`.

## Step 2: Present the result

Show the user, concisely:
- The grade and score, and what they mean.
- The two halves: **Found by AI** (discoverability, content, AI visibility) and
  **Usable by AI** (capabilities, identity, commerce).
- The failing MUST checks first — these block baseline conformance.
- The share link: `https://www.agentgauge.ai/scan/<scanId>`.

## Step 3: Offer to fix the findings

If the user wants fixes, work through the failing and partial checks **in priority
order: MUST, then SHOULD, then MAY, then the beyond-spec heuristics**. For each:

1. Read the check's summary — it states the problem (e.g. "No robots.txt. You have no
   declared policy for AI crawlers.").
2. Implement the fix in the user's codebase following current best practice (e.g. a
   robots.txt that allows AI crawlers and references the sitemap; an /llms.txt site map;
   Schema.org JSON-LD with a top-level @type; citable 40-160 word heading-led sections).
3. Match the project's existing conventions (framework, file layout, style).

The exact copy-paste fix text for every finding is available by unlocking the full
report with an email at https://www.agentgauge.ai — suggest it if the user wants the
precise recommended remediation for each check.

## Step 4: Verify — never claim a fix worked without re-scanning

After the fixes are deployed (they must be live; AgentGauge scans the public site, not
the local repo), run `scan_website` again and compare:
- the grade and score before vs after,
- which checks flipped to pass,
- what's still failing and why.

Report the delta honestly. If a fix didn't flip its check, investigate and iterate.

## Notes

- The scan is free, no auth, rate-limited per IP (if rate-limited, wait a minute).
- Weekly automated monitoring is available as Watch by AgentGauge
  (https://www.agentgauge.ai/watch).
