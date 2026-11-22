# NameOcean MCP server

[![mcptry](https://mcptry.com/badge/net.nameocean/mcp)](https://mcptry.com/servers/net.nameocean/mcp)

NameOcean runs a remote [MCP](https://modelcontextprotocol.io) server at

```
https://nameocean.net/mcp
```

Connect Claude Code, Claude, Cursor, VS Code or any other MCP client once. You can then ask it to manage your NameOcean domains, DNS, SSL certificates, app servers, email, uptime monitors and webhooks. It has 90 tools. You sign in in the browser, so there is no key to paste and nothing to install.

This repository holds the documentation and the [MCP Registry](https://registry.modelcontextprotocol.io) entry (`net.nameocean/mcp`). The server itself runs on nameocean.net.

## Connect your client

**Claude Code**

```sh
claude mcp add --transport http nameocean https://nameocean.net/mcp
```

Then run `/mcp` in Claude Code, choose **nameocean** and sign in.

**Claude Desktop and claude.ai**

Go to Settings → Connectors → Add custom connector. Name it NameOcean and use the URL `https://nameocean.net/mcp`.

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "nameocean": {"url": "https://nameocean.net/mcp"}
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "nameocean": {"type": "http", "url": "https://nameocean.net/mcp"}
  }
}
```

**OpenCode** (`~/.config/opencode/opencode.json`)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "nameocean": {"type": "remote", "url": "https://nameocean.net/mcp", "enabled": true}
  }
}
```

**With the NameOcean CLI:** `nameocean mcp install` finds Claude Code, Cursor, OpenCode and VS Code on your computer and adds the server to each ([install the CLI](https://nameocean.net/developers/cli/)).

**Clients without browser sign-in:** send an [API key](https://nameocean.net/dashboard/api/) in a header, `Authorization: Bearer no_live_…`. The tools then get exactly the key's scopes, so a key with only `dns:read` can read DNS but not change it.

## Signing in

The first time, the client opens a NameOcean page in your browser. You sign in, enter your two-step login code and choose what the app may do: DNS, servers, email and so on. Billing is off unless you tick it. App connections and API keys need [two-step login](https://nameocean.net/dashboard/security/) on your account.

## What to ask

- "Point blog.example.com to 203.0.113.10 and turn on SSL."
- "Set up email for example.com: a mailbox for hello@, forward sales@ to my Gmail, and check the DNS."
- "Deploy this project to my server and show me the logs if it does not come up."
- "Move every record of example.com to the new server IP. Show me the plan first."
- "Watch https://example.com every minute and tell me what went wrong in the last outage."
- "Send a webhook to https://example.com/hooks when a domain is about to expire."

## What it can do

| Area | Tools |
|---|---|
| Account and domains | account and credit, domain availability and price, your domains, auto-renew and transfer lock, nameservers, cart and checkout link, renewals, transfers, DNSSEC, activity log |
| DNS | zones, records (add, change, delete), change history with undo, export, plan and apply a whole zone (DNS as code), zone transfers to your own secondaries |
| SSL | certificate status, free wildcard Let's Encrypt certificates |
| App servers | servers and plans, create and resize (with a price confirmation), start, stop, restart, ports, domain binding with HTTPS, backups and restore, apps, logs, env variables, SSH keys, deploys |
| Email | mail domains, mailboxes, aliases, DNS check (MX, SPF, DKIM, DMARC), free forwarding, received and sent mail, sending email |
| Monitoring | uptime monitors (http, tcp, cron), incidents, public status pages |
| Webhooks | endpoints, test events, delivery log |

The full list with every tool's description is at [nameocean.net/developers/mcp](https://nameocean.net/developers/mcp/#tools).

The server also has prompts (`deploy-app`, `point-domain`, `fix-app`, `check-email-dns`) and resources (`nameocean://guides/deploy`, `nameocean://guides/dns`, `nameocean://servers/{server}/deploy-guide`).

## What your AI can and cannot do

- It cannot register or renew a domain or add credit. It checks availability and price, puts the domain in your cart and gives you the checkout link.
- Creating or resizing an app server is billed monthly, so it takes two calls: the first returns the price for the AI to show you, and the second runs only with your yes.
- Mailbox passwords and env variable values never reach the model; you read them in the dashboard.
- Tools that delete, stop or overwrite are marked as destructive, so clients that confirm such tools ask you first.
- Everything it changes is in [Dashboard → Activity](https://nameocean.net/dashboard/activity/), with the app that did it. You can disconnect an app at any time in [Dashboard → Connected apps](https://nameocean.net/dashboard/connected-apps/).

## Protocol

Streamable HTTP, stateless, protocol versions 2025-11-25, 2025-06-18 and 2025-03-26. Sign-in uses OAuth 2.1 with PKCE, dynamic client registration and the device flow; the metadata is at `https://nameocean.net/.well-known/oauth-protected-resource`. The tools call the same code as the [REST API](https://nameocean.net/developers/api/), so ownership checks, scopes and rate limits are the same.

## Help

Problems or ideas: [open an issue](https://github.com/NameOcean/mcp/issues) or write to support from your [NameOcean dashboard](https://nameocean.net/dashboard/).
