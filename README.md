# mailsimple-marketplace

This is the public **Claude Code marketplace** source for [MAILsimple by CCMS Hosting](https://ccmssolutions.com/mailsimple/).

It contains only the plugin manifests that tell Claude Code how to connect to the MAILsimple service — no server code, no secrets.

## Install

In Claude Code:

```
/plugin marketplace add ccms-inc/mailsimple-marketplace
/plugin install mailsimple-connector@ccms-hosting
```

Then set your endpoint and access key (from your account at [ccmssolutions.com/mailsimple/](https://ccmssolutions.com/mailsimple/)):

```bash
export MAILSIMPLE_MCP_URL="https://mailsimple.ccmssolutions.com/"
export MAILSIMPLE_CONNECTOR_KEY="your-key-here"
```

Restart Claude Code. See the [plugin README](plugins/mailsimple-connector/README.md) for full instructions.

## About

MAILsimple lets Claude read, search, organize, draft, and send mail across your IMAP/SMTP and Outlook/Office 365 mailboxes through a remote MCP server operated by CCMS Hosting. Your credentials stay on the server, encrypted at rest.

- Product page: https://ccmssolutions.com/mailsimple/
- Docs: https://ccmssolutions.com/mailsimple/docs/
- Privacy: https://ccmssolutions.com/mailsimple/privacy/
- Terms: https://ccmssolutions.com/mailsimple/terms/
- Support: support@ccmssolutions.com
