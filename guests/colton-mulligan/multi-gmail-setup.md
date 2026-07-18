# Multiple Gmail accounts in Claude: the setup that actually works

Promised at Session 001. The official Gmail connector handles exactly one Google account. This gets you as many as you want, each addressed by a friendly alias ("work", "personal"), usable from Claude Code, Claude Desktop, Cursor, or any MCP client.

The heavy lifting is an open-source local MCP server: **[Vinksj/claude-gmail-multi](https://github.com/Vinksj/claude-gmail-multi)**. Tokens stay on your machine. 22 tools: search, read, draft, send, labels, attachments.

## 1. Install (5 min)

```bash
git clone https://github.com/Vinksj/claude-gmail-multi.git
cd claude-gmail-multi
npm install
npm run build
```

Needs Node ≥ 20.

## 2. One-time Google OAuth app (~10 min, free)

You create one "app" in Google Cloud; it then works for every account you connect.

1. [console.cloud.google.com](https://console.cloud.google.com) → new project (call it `gmail-mcp`).
2. APIs & Services → Library → **Gmail API** → Enable.
3. APIs & Services → OAuth consent screen → User type **External** → app name + your email → save through the Scopes and Test users pages.
4. **Publish the app to production** (OAuth consent screen → Publishing status → Publish). Skipping this means Google kills your refresh token every 7 days. You'll click through a one-time "Google hasn't verified this app" warning per account (Advanced → Continue). Normal for a personal app.
5. Credentials → Create Credentials → OAuth client ID → **Desktop app** → download the JSON.
6. Save it as `~/.gmail-mcp/credentials.json`.

## 3. Connect accounts

Follow the repo README's `add_account` flow: each account gets a browser OAuth dance once, then an alias. Repeat per inbox.

## 4. Wire into Claude

Add the server to your MCP config (Claude Desktop config or `claude mcp add` for Claude Code) per the repo README. Every tool takes an `account` parameter, so "check my personal inbox" and "draft from work" just work in the same conversation.

## Gotchas I hit

- Google Workspace security tightening broke some older gcloud-CLI approaches; this OAuth-desktop-app path is the one that survived.
- Publish-to-production (step 2.4) is the step everyone skips and then wonders why it dies a week later.
- Keep the server local. Do not deploy your token store anywhere.

Questions → colton@foxfuelcreative.com
