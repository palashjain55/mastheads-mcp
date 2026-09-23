# Mastheads MCP server

Read and write your own AI newsroom over the Model Context Protocol: articles, their sources, the checks they passed, and the editor of record.

This is a remote server. There is nothing to install: it runs at `https://api.mastheads.app/v1/mcp` (streamable HTTP) and any MCP client connects to it with a Mastheads API key. This repository holds the documentation only; the server's code lives with the product.

- Product: https://mastheads.app
- Connect guides per client (Claude, ChatGPT, Cursor, Codex): https://mastheads.app/integrations
- Official MCP Registry entry: `app.mastheads/newsroom` (https://registry.modelcontextprotocol.io/v0.1/servers/app.mastheads%2Fnewsroom/versions)

## Connect

Create an API key in the Mastheads dashboard under Settings > Developer. A read-only key is enough for the eleven read tools; a Full access key adds the five write tools.

Claude Code, Cursor and VS Code use this shape:

```json
{
  "mcpServers": {
    "mastheads": {
      "type": "http",
      "url": "${MCP_URL}",
      "headers": { "Authorization": "Bearer mh_live_..." }
    }
  }
}
```

Claude Desktop and claude.ai connect by signing in with your Mastheads account instead of pasting a key. ChatGPT and Codex are covered on the integrations page linked above.

## Tools

Read (a read-only key is enough):

- `list_domains`: The publications on your account
- `list_articles`: Articles on a domain. No body - that is get_article's job, and returning 100 bodies blew out the context a list exists to save
- `get_article`: One article, in full
- `get_article_sources`: The sources it was written from, the desk verdicts recorded against it, and the editor of record
- `read_jobs`: What is running now and how far along
- `read_quota`: Articles left this period
- `read_settings`: How a domain is configured
- `read_subscription`: The plan and its limits
- `propose_generate`: Returns a confirm card and a link into the dashboard. It generates nothing
- `get_research_report`: A finished research report
- `get_audit`: A finished domain audit, and any one of its six views

Write (needs a key created with Full access):

- `write_article`: Start one article
- `write_articles`: Start a batch, up to 100 rows
- `run_research`: Start a research report
- `run_audit`: Start a domain audit
- `publish_article`: Put a finished article live on the domain's connected site

`propose_generate` never generates anything on its own; it returns a confirm card and a link into the dashboard. Writes that spend articles are the five above, and only a Full access key sees them.

## What an article carries

Every article written by Mastheads keeps the sources it was written from, the checks it passed, its named byline and the editor of record. `get_article_sources` returns that record for one article, which is what makes the newsroom readable by an agent rather than only by a person.

## Support

hello@mastheads.app
