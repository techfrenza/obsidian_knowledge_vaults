**Concept**: Cloudflare's 2-Tool Paradox

**Hook Insight**: Cloudflare had 2,594 API endpoints. The "obvious" AI integration = one MCP tool per endpoint = 244K tokens just for tool definitions = context window explodes before the conversation starts. Their solution: compress to 2 tools (search + execute) — agent writes JavaScript to call APIs. Token count: 1M+ → ~1,000. The counterintuitive insight: **giving agents FEWER tools sometimes makes them MORE capable**, because code generation accuracy far exceeds tool selection accuracy at scale. Anthropic's own data: 134K token tool definitions → Opus 4 accuracy drops to 49%.

**Wiki Link**: AI_Native_Tool_Design — 3 AI design constraints (no memory, can't browse, needs precision)
