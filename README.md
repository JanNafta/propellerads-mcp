<div align="center">

# PropellerAds MCP Server

**Democratizing Programmatic Advertising with AI**

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/downloads/)
[![MCP](https://img.shields.io/badge/MCP-1.0-00B4D8?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZD0iTTEyIDJMMiA3djEwbDEwIDUgMTAtNVY3TDEyIDJ6IiBmaWxsPSJ3aGl0ZSIvPjwvc3ZnPg==&logoColor=white)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)]()
[![GitHub Stars](https://img.shields.io/github/stars/JanNafta/propellerads-mcp?style=for-the-badge&logo=github)](https://github.com/JanNafta/propellerads-mcp/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/JanNafta/propellerads-mcp?style=for-the-badge&logo=github)](https://github.com/JanNafta/propellerads-mcp/network/members)

Let AI assistants like **Claude** manage your advertising campaigns on **PropellerAds** automatically.

[Quick Start](#-quick-start) &bull; [Available Tools](#-available-tools) &bull; [Usage Examples](#-usage-examples) &bull; [MCP Configuration](#-mcp-configuration)

</div>

---

## What is this?

PropellerAds MCP is a **Model Context Protocol server** that connects AI assistants (Claude, and any MCP-compatible client) directly to the [PropellerAds](https://propellerads.com) advertising platform API. Instead of manually logging into dashboards, pulling reports, and clicking through settings, you simply **talk to your AI assistant in plain English** and it handles everything for you.

Create campaigns, analyze performance, blacklist underperforming zones, find scaling opportunities, compare time periods -- all through natural conversation.

**Built for:**
- Media Buyers and Performance Marketers
- iGaming and App Install Affiliates
- Growth Hackers and Digital Agencies
- Anyone running PropellerAds campaigns who wants to work faster

---

## Features

- **Full Campaign Lifecycle** -- Create, update, start, stop, and clone campaigns without leaving your chat
- **Real-Time Performance Analytics** -- Impressions, clicks, conversions, CTR, CVR, CPC, CPA, and ROI calculated automatically
- **Period-over-Period Comparison** -- Compare any two date ranges side by side with trend indicators
- **Zone-Level Optimization** -- Find underperforming zones wasting budget and top zones worth whitelisting
- **Automated Blacklisting** -- One command to identify and blacklist bad zones (with dry-run safety mode)
- **Scaling Intelligence** -- Automatically find campaigns with strong ROI and conversion volume ready to scale
- **Creative Performance Breakdown** -- See which creatives drive results and which need replacement
- **Secure by Design** -- API token stored in environment variables, never exposed in conversation
- **Dry Run Safety** -- Destructive operations default to preview mode before executing

---

## Available Tools

### Campaign Management

| Tool | Description | Required Parameters |
|------|-------------|-------------------|
| `list_campaigns` | List all campaigns with optional filters | -- |
| `get_campaign_details` | Get complete campaign info (targeting, creatives, settings) | `campaign_id` |
| `create_campaign` | Create a new advertising campaign | `name`, `ad_format`, `countries`, `daily_budget`, `bid`, `target_url` |
| `update_campaign` | Modify campaign settings (budget, bid, name, status) | `campaign_id` |
| `start_campaigns` | Activate one or more paused campaigns | `campaign_ids` |
| `stop_campaigns` | Pause one or more active campaigns | `campaign_ids` |
| `clone_campaign` | Duplicate an existing campaign | `campaign_id` |

**Filters for `list_campaigns`:** `status` (active/paused/pending/rejected), `ad_format` (push/onclick/interstitial/in-page-push), `name` (partial match)

### Statistics & Analytics

| Tool | Description | Required Parameters |
|------|-------------|-------------------|
| `get_performance_report` | Detailed stats with computed metrics (CTR, CVR, CPC, CPA, ROI) | -- |
| `get_campaign_performance` | Performance summary for a specific campaign | `campaign_id` |
| `compare_periods` | Compare two time periods with change indicators | `period1_from`, `period1_to`, `period2_from`, `period2_to` |
| `get_zone_performance` | Zone/placement-level analytics, sortable | -- |
| `get_creative_performance` | Creative-level performance breakdown | -- |

**Common optional params:** `date_from`, `date_to` (YYYY-MM-DD, defaults to last 7 days), `campaign_id`, `group_by` (date/campaign/zone/country/creative/device_type/browser/os)

### Optimization

| Tool | Description | Required Parameters |
|------|-------------|-------------------|
| `find_underperforming_zones` | Find zones spending money without converting (blacklist candidates) | `campaign_id` |
| `find_top_zones` | Find best-performing zones (whitelist candidates) | `campaign_id` |
| `find_scaling_opportunities` | Find campaigns ready for scaling (high ROI + volume) | -- |
| `auto_blacklist_zones` | Find and blacklist bad zones in one step (dry run by default) | `campaign_id` |

### Targeting

| Tool | Description | Required Parameters |
|------|-------------|-------------------|
| `add_to_whitelist` | Add zones to a campaign's whitelist | `campaign_id`, `zone_ids` |
| `add_to_blacklist` | Add zones to a campaign's blacklist | `campaign_id`, `zone_ids` |

### Account

| Tool | Description | Required Parameters |
|------|-------------|-------------------|
| `get_balance` | Check current account balance | -- |
| `get_available_countries` | List all countries available for targeting | -- |
| `get_ad_formats` | List available ad formats (push, onclick, etc.) | -- |

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Runtime | Python 3.10+ |
| Protocol | [Model Context Protocol (MCP)](https://modelcontextprotocol.io) 1.0 |
| HTTP Client | [httpx](https://www.python-httpx.org/) |
| Validation | [Pydantic](https://docs.pydantic.dev/) v2 |
| API | [PropellerAds SSP API v5](https://ssp-api.propellerads.com/v5) |
| Build System | [Hatchling](https://hatch.pypa.io/) |
| Transport | stdio (standard MCP transport) |

---

## Quick Start

### Prerequisites

1. **PropellerAds Account** with API access
   - Minimum requirement: $1,000 total spend or deposit
   - Get your API token: https://ssp.propellerads.com/#/app/profile
2. **Python 3.10+**
3. **Claude Desktop** or **Claude Code** (or any MCP-compatible client)

### Installation

#### Option 1: Install from PyPI (Recommended)

```bash
pip install propellerads-mcp
```

#### Option 2: Install from source

```bash
git clone https://github.com/JanNafta/propellerads-mcp.git
cd propellerads-mcp
pip install -e .
```

### Set your API token

Create a `.env` file in the project root or export the environment variable:

```bash
export PROPELLERADS_API_TOKEN="your_api_token_here"
```

---

## Usage Examples

### Campaign Management

```
"Show me all my active campaigns sorted by ROI"

"Create a push campaign for gaming offers in Brazil with $100 daily budget"

"Pause all campaigns with negative ROI in the last 7 days"

"Clone my best performing campaign to Mexico, Colombia, and Peru"
```

### Performance Analysis

```
"What's my campaign performance for the last week?"

"Compare this week's performance vs last week"

"Show me the top 10 zones by conversions for campaign 12345"

"Which creatives have CTR below 0.5%?"
```

### Optimization Workflows

```
"Find all zones spending over $50 without conversions and blacklist them"

"Show me campaigns ready for scaling -- ROI above 50% with at least 10 conversions"

"Find top performing zones for my dating campaigns and add them to a whitelist"
```

### Daily Optimization Routine

```
1. "Show me yesterday's performance for all campaigns"
2. "Find and blacklist underperforming zones across all campaigns"
3. "Which campaigns are ready for scaling?"
4. "Increase budget by 50% for profitable campaigns"
```

---

## MCP Configuration

### Claude Desktop

Add to your Claude Desktop config file:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "propellerads": {
      "command": "python",
      "args": ["-m", "propellerads_mcp"],
      "env": {
        "PROPELLERADS_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

Restart Claude Desktop after saving the configuration.

### Claude Code

Add the MCP server to Claude Code using the CLI:

```bash
claude mcp add propellerads -- python -m propellerads_mcp
```

Make sure `PROPELLERADS_API_TOKEN` is set in your shell environment before launching Claude Code.

### Other MCP Clients

This server uses **stdio transport**, the standard MCP communication method. Any MCP-compatible client can connect by spawning the process:

```bash
python -m propellerads_mcp
```

The server reads `PROPELLERADS_API_TOKEN` from the environment. Pass it via the `env` configuration of your MCP client or set it in your shell.

---

## Project Structure

```
propellerads-mcp/
├── src/
│   └── propellerads_mcp/
│       ├── __init__.py       # Package init, version, exports
│       ├── __main__.py       # Module entry point (python -m)
│       ├── client.py         # PropellerAds API client (httpx-based)
│       └── server.py         # MCP server, tool definitions & handlers
├── .env.example              # Environment variable template
├── .gitignore
├── LICENSE                   # MIT License
├── pyproject.toml            # Build config, dependencies, metadata
└── README.md
```

---

## Security & Permissions

| Aspect | Details |
|--------|---------|
| **Authentication** | Bearer token via environment variable (never hardcoded) |
| **Read operations** | Executed without additional confirmation |
| **Write operations** | Require explicit user intent (create, update, start, stop, blacklist) |
| **Auto-blacklist** | Defaults to `dry_run: true` -- preview before executing |
| **Rate limiting** | Respects PropellerAds API rate limits |
| **No data storage** | The server is stateless; no data is persisted locally |

---

## Contributing

Contributions are welcome! Here is how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/my-feature`)
3. **Commit** your changes (`git commit -m "Add my feature"`)
4. **Push** to your branch (`git push origin feature/my-feature`)
5. **Open** a Pull Request

For bugs and feature requests, please [open an issue](https://github.com/JanNafta/propellerads-mcp/issues).

---

## Author

**Jan Naftanaila** -- Media Buyer & AI Automation Specialist

Building tools that bridge the gap between AI and programmatic advertising. Focused on making adtech accessible, automated, and intelligent.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-jannafta-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/jannafta-programmatic-performance-dsp-ssp-rtb/)
[![Website](https://img.shields.io/badge/Website-jannafta.com-000000?style=flat-square&logo=safari&logoColor=white)](https://www.jannafta.com/ia)
[![GitHub](https://img.shields.io/badge/GitHub-JanNafta-181717?style=flat-square&logo=github)](https://github.com/JanNafta)

---

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

<div align="center">

**[PropellerAds MCP](https://github.com/JanNafta/propellerads-mcp)** -- Open source. Built for the programmatic advertising community.

</div>
