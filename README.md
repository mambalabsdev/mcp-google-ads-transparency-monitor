# Google Ads Transparency Monitor MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-google-ads-transparency-monitor)](https://lightnow.ai/servers/com.mambabuilt/mcp-google-ads-transparency-monitor) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-google-ads-transparency-monitor/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-google-ads-transparency-monitor) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-google-ads-transparency-monitor%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-google-ads-transparency-monitor&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-google-ads-transparency-monitor)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-ads-transparency-monitor) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-google-ads-transparency-monitor)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-ads-transparency-monitor) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-google-ads-transparency-monitor)](https://github.com/mambalabsdev/mcp-google-ads-transparency-monitor/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-google-ads-transparency-monitor)

<a href="https://mcprush.com/to/google-ads-transparency-monitor-mcp?ref=badge"><img width="380" height="200" src="https://mcprush.com/badge/google-ads-transparency-monitor-mcp/card.svg" alt="Google Ads Transparency Monitor MCP server" /></a>

An MCP server for the Mamba Labs Google Ads Transparency Monitor actor on Apify. Give it a company domain or an advertiser name. Each run reads Google's public Ads Transparency Center and returns one row per ad, with the format, first and last shown dates, a creative link, and the ad's page, plus a company summary row with Google's total ad count band.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

List the Google ads a company runs. Give a company domain (for example stripe.com) or an advertiser name, or lists of either. You get one row per ad with the advertiser, ad ID, format (text, image, or video), first and last shown dates, a creative link, and the ad's Transparency Center page, plus one summary row per input that says whether the company runs Google ads, how many (Google's own count band, for example 8,000 to 9,000), and in which formats. Rows with no ads, blocked requests, or a changed Google page come back free with a `row_status` saying so.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-google-ads-transparency-monitor": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-google-ads-transparency-monitor"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_google_ads` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Does notion.so run Google ads? How many, and in which formats?"
- "Show me the 10 most recent video ads HubSpot ran in the US."
- "For stripe.com, ramp.com, and brex.com, which ones advertised on Google in the last 30 days?"

## Tool

`get_google_ads`. One tool. Example arguments:

```json
{
  "company_domain": "stripe.com",
  "max_ads": "20",
  "since_days": "90"
}
```

## Inputs

Every buyer facing input of the live actor, unchanged. Omitted inputs take the actor's own defaults. Give one of `company_domain`, `advertiser_name`, `domains`, or `advertiser_names`.

| Input | Type | Required | Description |
|---|---|---|---|
| `company_domain` | string | no | A company website domain, for example stripe.com. |
| `advertiser_name` | string | no | An advertiser name as Google verifies it, for example Stripe, Inc. |
| `domains` | array | no | Several company domains. Up to 500 inputs per run. |
| `advertiser_names` | array | no | Several advertiser names. |
| `region` | string | no | anywhere, or a two letter country code such as US, GB, DE, or FR. Default anywhere. |
| `formats` | array | no | text, image, video. Omit for all formats. |
| `max_ads` | string | no | Ad rows per input, 1 to 1000. Default 50. |
| `since_days` | string | no | Only ads last shown in this many days; 0 for all. Default 90. |
| `include_summary` | string | no | true or false. Default true. |
| `max_charge_usd` | string | no | Optional cost cap for the run. |
| `skipCache` | string | no | true reads fresh results instead of the free 24 hour cache. Default false. |
| `proxy_mode` | string | no | auto, direct, or residential. Default auto. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `ad-found`, per ad row returned | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `company-summary`, per input with ads | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Rows with no ads, blocked rows, changed source rows, error rows, and cached rows are free.

## Your keys

The actor needs no provider key. It reads Google's public Ads Transparency Center and holds no key of its own. If a run uses the Apify proxy, that traffic adds nothing to your price: Mamba Labs pays the platform costs of the run. `proxy_mode` is a reliability setting.

## Full actor documentation

[apify.com/mambalabs/google-ads-transparency-monitor](https://apify.com/mambalabs/google-ads-transparency-monitor)

## License

MIT. See [LICENSE](LICENSE).
