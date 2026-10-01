# MAILsimple — Mail (IMAP/SMTP) Connector for Claude

Give Claude secure access to your mailboxes — read, search, organize, draft, and
send mail across any mix of IMAP/SMTP and Outlook/Office 365 accounts — through
**MAILsimple by CCMS Hosting**, a remote MCP server that runs on CCMS
infrastructure so your credentials never live in Claude.

This plugin is a thin, secret-free pointer to your MAILsimple endpoint. You
supply one value — your **API key** (`csk_…`) — and Claude connects. (The
endpoint URL is built in; see [Install](#install) to override it.)

> **You need a MAILsimple account.** This plugin connects to the MAILsimple
> service operated by CCMS Hosting; it does not include the server. Full
> walkthrough: **https://mailsimple.ccmssolutions.com/setup-guide**. In short:
>
> 1. **Sign up and choose a plan** at **https://account.ccmssolutions.com**.
> 2. **Add your mailboxes** at **https://mailsimple-mcp.ccmssolutions.com** —
>    sign in and connect each IMAP or Outlook account Claude may use.
> 3. **Generate a key** at **https://account.ccmssolutions.com** under
>    **API Keys** (choose MAILsimple). It starts with `csk_` and is shown once.

## Pricing

- **Free** — 1 connected mailbox (Outlook or IMAP).
- **Pro** — unlimited mailboxes, **$9/month or $90/year**.

The plan limits the number of mailboxes only; every tool below works on every
plan. Plans are managed at https://account.ccmssolutions.com.

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

In Claude Code:

```
/plugin marketplace add ccms-inc/mailsimple-marketplace
/plugin install mailsimple-connector@ccms-hosting
```

### 2. Provide your key (and, optionally, the endpoint)

The bundled MCP server config ([`.mcp.json`](.mcp.json)) reads its settings by
**environment-variable interpolation**, so no secret is ever stored in the
plugin:

| Variable | Value | Notes |
|----------|-------|-------|
| `MAILSIMPLE_CONNECTOR_KEY` | `csk_…` (from account.ccmssolutions.com → API Keys) | **Required.** Sent as `Authorization: Bearer <key>`. Treat as a password. |
| `MAILSIMPLE_MCP_URL` | `https://mailsimple-mcp.ccmssolutions.com/mcp` | Optional; this is the default. **HTTPS and the `/mcp` path** — the host root serves the sign-in app, not the connector. |

Set them in the environment Claude Code runs in:

```bash
# macOS / Linux (add to your shell profile)
export MAILSIMPLE_MCP_URL="https://mailsimple-mcp.ccmssolutions.com/mcp"
export MAILSIMPLE_CONNECTOR_KEY="csk_your-key-here"
```

```powershell
# Windows PowerShell (persist for your user)
setx MAILSIMPLE_MCP_URL "https://mailsimple-mcp.ccmssolutions.com/mcp"
setx MAILSIMPLE_CONNECTOR_KEY "csk_your-key-here"
```

Then restart Claude Code so the plugin's MCP server picks up the values.

### 3. Verify

Ask Claude: *"List my mail accounts."* It should call `list_accounts` and return
your mailboxes. If you get an auth error, re-check the key. A `404` means the URL
is missing the `/mcp` path; a `402` means the subscription lapsed — settle it on
the **Billing** page at https://account.ccmssolutions.com.

---

## Other MCP clients

MAILsimple is a standard remote MCP server (Streamable HTTP). It works with any
MCP client that can send an `Authorization: Bearer` header: point the client at
`https://mailsimple-mcp.ccmssolutions.com/mcp` and send your `csk_…` key as
`Authorization: Bearer csk_…`.

The custom-connector flows in the Claude Desktop and claude.ai apps expect an
OAuth sign-in, which MAILsimple does not offer yet, so pasting a key there will
not connect. Use Claude Code with this plugin.

---

## Authentication & privacy

- **Header auth.** Your key is sent as an `Authorization: Bearer` header — not in
  the URL — so it doesn't leak into web-server logs, proxies, or history.
- **Your credentials stay on the server.** Your mailbox passwords / OAuth tokens
  live on CCMS's MAILsimple server, encrypted at rest; they are never placed in
  this plugin and never sent to Anthropic. See the
  [privacy policy](https://mailsimple.ccmssolutions.com/privacy-policy).
- **HTTPS only.** The endpoint is always `https://`.

---

## Security notes

- **No secrets in this plugin.** The key comes from your environment at runtime.
  Never hardcode a real key into `.mcp.json`.
- **Your key is a bearer credential.** Anyone with it can reach the mailboxes on
  your account. If it's ever exposed, revoke and regenerate it under **API Keys**
  at https://account.ccmssolutions.com — the old key stops working at once.
- **Scope per account.** Each mailbox has its own permissions at
  https://mailsimple-mcp.ccmssolutions.com — untick **"Allow organizing"** for a
  read-only mailbox, and leave SMTP disabled so it can't send.

See the [privacy policy](https://mailsimple.ccmssolutions.com/privacy-policy) and
[terms of service](https://mailsimple.ccmssolutions.com/terms-of-service). For
security questions, email support@ccmssolutions.com.

---

## Branding

`assets/icon.svg` is the plugin icon — the CCMS dot-triangle reading as mail
converging into an envelope (CCMS red `#ED1C24`). It is a vector master that
scales to any size; PNG rasters are alongside it.

---

_MAILsimple is built and operated by **CCMS Hosting** (Complete Content
Management Services, Inc.). Support: support@ccmssolutions.com · +1-954-693-6422._
