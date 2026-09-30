# mailsimple-marketplace

This is the public **Claude Code marketplace** source for [MAILsimple by CCMS Hosting](https://mailsimple.ccmssolutions.com).

It contains only the plugin manifests that tell Claude Code how to connect to the MAILsimple service — no server code, no secrets.

## Install

In Claude Code:

```
/plugin marketplace add ccms-inc/mailsimple-marketplace
/plugin install mailsimple-connector@ccms-hosting
```

Then set your API key (a `csk_…` key from **API Keys** in your account at [account.ccmssolutions.com](https://account.ccmssolutions.com)):

```bash
export MAILSIMPLE_CONNECTOR_KEY="csk_your-key-here"
# Optional; this is the built-in default:
export MAILSIMPLE_MCP_URL="https://mailsimple-mcp.ccmssolutions.com/mcp"
```

Restart Claude Code. See the [plugin README](plugins/mailsimple-connector/README.md) for full instructions, or the [setup guide](https://mailsimple.ccmssolutions.com/setup-guide).

Any other MCP client that can send an `Authorization: Bearer` header can connect to `https://mailsimple-mcp.ccmssolutions.com/mcp` with the same key.

## About

MAILsimple lets Claude read, search, organize, draft, and send mail across your IMAP/SMTP and Outlook/Office 365 mailboxes through a remote MCP server operated by CCMS Hosting. Your credentials are stored encrypted on the server, never in the plugin.

**Pricing:** Free for 1 mailbox; Pro is $9/month or $90/year for unlimited mailboxes.

- Product page: https://mailsimple.ccmssolutions.com
- Setup guide: https://mailsimple.ccmssolutions.com/setup-guide
- Privacy: https://mailsimple.ccmssolutions.com/privacy-policy
- Terms: https://mailsimple.ccmssolutions.com/terms-of-service
- Support: support@ccmssolutions.com
