# hlist.cc

A 4-language (EN / 中文 / 日本語 / 한국어) directory of AI tools, updated daily.
Every listing includes its category, pricing tier (verified against the official pricing page),
and a **China-accessibility field** (`ok` / `unstable` / `blocked` / `unknown`) —
the only AI tools directory that publishes it.

Live: **https://hlist.cc**

## For AI agents

The whole corpus is machine-readable. Three exits, all public and keyless:

```bash
# MCP endpoint — search 300+ AI tools
curl -X POST https://hlist.cc/api/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"search_ai_tools","arguments":{"category":"code"}}}'
```

- **MCP server**: `https://hlist.cc/api/mcp` (no key, no rate limit) — tools: `search_ai_tools`, `get_tool`, `list_categories`; optional `lang` param for en/zh/ja/ko descriptions
- **Full dataset**: `https://hlist.cc/api/v1/tools.json` — one record per tool (slug, category, pricing, cn_access, GitHub stars, verified date)
- **Subsets**: `https://hlist.cc/api/v1/tools.json?cn_access=ok` / `?category=video` / `?pricing_tier=free` (combinable)
- **LLM index**: `https://hlist.cc/llms.txt` (with a "citable facts" block at the top)
- **Usage stats**: `https://hlist.cc/api/v1/mcp-stats.json` (which agents are calling, by day)

Listed in the official MCP Registry, Glama, and Smithery.

## Data pipeline

This repo powers the site:

- **Front**: Astro (static, ~2,200 pages) on Cloudflare Pages, 4 languages with hreflang
- **Collect**: nightly pipeline (06:00 UTC on a dedicated server) pulls candidates from GitHub Trending, HuggingFace Spaces, and HN, runs a verification gate (website resolves + screenshot + data field confirmed), generates 4-language pages, builds, and deploys with regression checks
- **Data exits**: `llms.txt` / `tools.json` / MCP are regenerated at build time from the same content
- **User data**: D1 (submissions, votes, comments, AI-crawler + MCP hit logs)
- **IndexNow**: pushed on every deploy

## Methodology (short version)

A tool is published only after its official website resolves, a screenshot is captured,
and at least one data field (pricing / category / description) is confirmed.
Pricing is verified against the vendor's official pricing page, not scraped from aggregators.
The China-accessibility field is a point-in-time probe from a mainland-China egress,
so it is a 4-value field (`ok` / `unstable` / `blocked` / `unknown`), not a yes/no.
Full method: https://hlist.cc/about/

## Sponsor

Featured listings, expedited review, and newsletter sponsorship: https://hlist.cc/sponsor/
