# Y Combinator Startup Jobs MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-yc-startup-jobs)](https://www.npmjs.com/package/@mambalabsdev/mcp-yc-startup-jobs) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-yc-startup-jobs)](https://github.com/mambalabsdev/mcp-yc-startup-jobs/blob/main/LICENSE)

An MCP server for the Mamba Labs Y Combinator Startup Jobs actor on Apify. YC startup jobs as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes roles, YC company slugs, YC batches, or YC industries, and returns one row per open job on Work at a Startup with title, company, YC batch, location, salary, equity, visa, and experience. Job details add the full description and skills.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-yc-startup-jobs": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-yc-startup-jobs"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_yc_startup_jobs` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List open engineering jobs at Y Combinator startups with salaries."
- "Find every open job at hiring companies from YC batch S25."

## Tool

`get_yc_startup_jobs`. One tool. Example arguments:

```json
{
  "batches": [
    "S25"
  ],
  "max_jobs_per_source": 20,
  "max_companies_per_source": 5
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `roles` | array | no | Work at a Startup role pages: eng, design, product, science, sales, marketing, support, operations, recruiting, finance, or legal. Logged out, each role page lists the newest 30 jobs for that role. |
| `companies` | array | no | YC company slugs or URLs, for example nox-metals, https://www.workatastartup.com/companies/nox-metals, or https://www.ycombinator.com/companies/nox-metals. Returns every open job the company lists. |
| `batches` | array | no | YC batches, for example S25, W24, or Summer 2025. Reads every company YC's directory marks as hiring in that batch, up to the company limit, then each company's jobs. |
| `industries` | array | no | YC industry names as the YC directory uses them, for example Fintech, B2B, or Healthcare. Works like batches. |
| `job_urls` | array | no | Optional. Work at a Startup job pages, https://www.workatastartup.com/jobs/<number>. A job URL always returns a job detail row. |
| `max_jobs_per_source` | number | no | Jobs to return for each role, company, batch, or industry. Default 100, maximum 5000. A role page holds at most 30. |
| `max_companies_per_source` | number | no | Hiring companies to read for each batch or industry. Default 50, maximum 500. |
| `include_details` | boolean | no | yes reads each job page and adds the full description, skills, equity, visa, experience, and company details, charged as a job detail row instead of a job row. Default no. |
| `dedupe` | boolean | no | yes (default) returns each job once per run, so a job found by two sources is charged once. no returns it again on each source, charged each time. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount, counting the run start. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `job-row`, one job from a role or company page | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `job-detail-row`, one job with its job page read | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.006 | $0.006 | $0.006 | $0.006 |

Repeats, empty sources, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/yc-startup-jobs](https://apify.com/mambalabs/yc-startup-jobs)

## License

MIT. See [LICENSE](LICENSE).
