# MAILsimple — Mail (IMAP/SMTP) Connector for Claude

Give Claude secure access to your mailboxes — read, search, organize, draft, and
send mail across any mix of IMAP/SMTP and Outlook/Office 365 accounts (Gmail coming soon) — through
**MAILsimple by CCMS Hosting**, a remote MCP server that runs on CCMS
infrastructure so your credentials never live in Claude.

This plugin is a thin, secret-free pointer to your MAILsimple endpoint. You
supply two values CCMS gives you — your **endpoint URL** and your **access
key** — and Claude connects.

> **You need a MAILsimple account.** This plugin connects to a MAILsimple server
> operated by CCMS Hosting; it does not include the server. Get your endpoint URL
> and key at **https://ccmssolutions.com/mailsimple/** (or from your CCMS contact).

---

## What you get

Once connected, Claude can use these tools (per mailbox on your account):

| Tool | Type | What it does |
|------|------|--------------|
| `list_accounts`   | read  | List your configured mailboxes and what each can do. |
| `list_folders`    | read  | List folders for an account. |
| `search_messages` | read  | Search a folder (from/to/subject/since/before/attachments + free text). |
| `get_message`     | read  | Read one message (body + attachment names). Body is flagged untrusted. |
| `get_attachment`  | read  | Fetch one attachment (5 MiB cap). |
| `get_thread`      | read  | Pull the whole conversation a message belongs to. |
| `move_message`    | write | Move a message to another folder. |
| `set_flags`       | write | Mark read/unread, flagged/unflagged. |
| `delete_message`  | write | Mark deleted (recoverable); permanent only with `expunge`. |
| `create_draft`    | write | Compose and save to Drafts **without sending**. |
| `send_message`    | send  | Send mail from the account (fixed From identity). |
| `reply_message`   | send  | Reply in-thread to the original sender. |

Read tools run without a permission prompt; **write/send/delete tools always
prompt** (they carry `destructiveHint`). Per-account permissions are enforced on
the server regardless of what the model attempts.

---

## Install

### 1. Add the marketplace and plugin

```
/plugin marketplace add ccms-hosting/mailsimple-marketplace   # the public repo CCMS gives you
/plugin install mailsimple-connector@ccms-hosting
```

### 2. Provide your endpoint and key

The bundled MCP server config ([`.mcp.json`](.mcp.json)) reads two values by
**environment-variable interpolation**, so no secret is ever stored in the
plugin:

| Variable | Example | Notes |
|----------|---------|-------|
| `MAILSIMPLE_MCP_URL` | `https://mailsimple.ccmssolutions.com/` | The endpoint CCMS gives you. **HTTPS, trailing slash.** |
| `MAILSIMPLE_CONNECTOR_KEY` | (the key CCMS issues you) | Sent as `Authorization: Bearer <key>`. Treat as a password. |

Set them in the environment Claude Code runs in:

```bash
# macOS / Linux (add to your shell profile)
export MAILSIMPLE_MCP_URL="https://mailsimple.ccmssolutions.com/"
export MAILSIMPLE_CONNECTOR_KEY="the-key-ccms-gave-you"
```

```powershell
# Windows PowerShell (persist for your user)
setx MAILSIMPLE_MCP_URL "https://mailsimple.ccmssolutions.com/"
setx MAILSIMPLE_CONNECTOR_KEY "the-key-ccms-gave-you"
```

Then restart Claude Code so the plugin's MCP server picks up the values.

### 3. Verify

Ask Claude: *"List my mail accounts."* It should call `list_accounts` and return
your mailboxes. If you get an auth error, re-check the URL (HTTPS, trailing
slash) and the key.

---

## Authentication & privacy

- **Header auth.** Your key is sent as an `Authorization: Bearer` header — not in
  the URL — so it doesn't leak into web-server logs, proxies, or history.
- **Your credentials stay on the server.** Your mailbox passwords / OAuth tokens
  live on CCMS's MAILsimple server, encrypted at rest; they are never placed in
  this plugin and never sent to Anthropic. See the
  [privacy policy](../../docs/PRIVACY.md).
- **HTTPS only.** The endpoint is always `https://`.

> **CCMS-hosted, OAuth option.** CCMS also offers MAILsimple through Claude's
> **Settings → Connectors** using OAuth sign-in (no key to manage). Use whichever
> your CCMS plan provides; this plugin is the key-based install path.

---

## Security notes

- **No secrets in this plugin.** The key and URL come from your environment at
  runtime. Never hardcode a real key into `.mcp.json`.
- **Your key is a bearer credential.** Anyone with it can reach the mailboxes on
  your account. If it's ever exposed, ask CCMS to rotate it.
- **Scope per account.** Ask CCMS to set read-only or no-send on mailboxes where
  you want a safety margin.

See [`SECURITY.md`](../../SECURITY.md), the
[privacy policy](../../docs/PRIVACY.md), [terms](../../docs/TERMS.md), and
[acceptable use policy](../../docs/ACCEPTABLE-USE.md).

---

## Branding

`assets/icon.svg` is the plugin icon — the CCMS dot-triangle reading as mail
converging into an envelope (CCMS red `#ED1C24`). It is a vector master that
scales to any size; export PNG rasters from it for directory listings.

---

_MAILsimple is built and operated by **CCMS Hosting** (Complete Content
Management Services, Inc.). Support: support@ccmssolutions.com · +1-954-693-6422._
