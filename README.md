# mailsimple-marketplace

This is the public **Claude Code marketplace** source for [MAILsimple by CCMS Hosting](https://ccmshightech.com/mailsimple/).

It contains only the plugin manifests that tell Claude Code how to connect to the MAILsimple service — no server code, no secrets.

## Install

In Claude Code:

```
/plugin marketplace add ccmshightech/mailsimple-marketplace
/plugin install mailsimple-connector@ccms-hosting
```

Then set your endpoint and access key (from your account at [ccmshightech.com/mailsimple/](https://ccmshightech.com/mailsimple/)):

```bash
export MAILSIMPLE_MCP_URL="https://mailsimple.ccmshightech.com/"
export MAILSIMPLE_CONNECTOR_KEY="your-key-here"
```

Restart Claude Code. See the [plugin README](plugins/mailsimple-connector/README.md) for full instructions.

## About

MAILsimple lets Claude read, search, organize, draft, and send mail across your IMAP/SMTP and Outlook/Office 365 mailboxes through a remote MCP server operated by CCMS Hosting. Your credentials stay on the server, encrypted at rest.

- Product page: https://ccmshightech.com/mailsimple/
- Docs: https://ccmshightech.com/mailsimple/docs/
- Privacy: https://ccmshightech.com/mailsimple/privacy/
- Terms: https://ccmshightech.com/mailsimple/terms/
- Support: ccmshightech@gmail.com
