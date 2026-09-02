# PostZen plugin for Grok Build

Schedule and publish social media posts across 10 platforms — X (Twitter), Instagram, TikTok, LinkedIn, Facebook, YouTube, Threads, Pinterest, Bluesky, and Telegram — without leaving Grok Build.

The plugin connects Grok Build to the hosted [PostZen MCP server](https://mcp.postzen.dev) and adds workflow skills on top of it: drafting and adapting content per platform, uploading media, filling posting queues, connecting accounts, and building analytics reports.

## Installation

1. In Grok Build, open the marketplace with `/marketplace`, find **postzen**, and press `i` to install it. This installs the skills and the bundled PostZen MCP server.
2. Open the MCP servers tab with `/mcp`, select **postzen**, and press `i` to sign in. Your browser opens the PostZen dashboard, where you approve access and choose which profiles to expose. No API key to paste.
3. Once postzen shows **ready**, ask Grok to post something.

Don't have a PostZen account? Sign up at [postzen.dev](https://www.postzen.dev) and connect your social accounts in the [dashboard](https://app.postzen.dev) — or let Grok walk you through it with `/postzen-connect`.

### Headless / CI use

For non-interactive environments, skip OAuth and configure the server yourself with a PostZen API key (create one in the dashboard under Settings → API keys) instead of the plugin's bundled OAuth config:

```json
{
  "mcpServers": {
    "postzen": {
      "type": "http",
      "url": "https://mcp.postzen.dev/mcp",
      "headers": { "Authorization": "Bearer pzn_your_api_key" }
    }
  }
}
```

## Skills

| Skill | What it does |
|---|---|
| `/postzen-post` | Draft, adapt, and publish or schedule a post across platforms, with media uploads, queue slots, and best-time-to-post suggestions |
| `/postzen-queue` | Set up recurring posting slots (at your best-performing times), preview upcoming posts, and fill the queue |
| `/postzen-analytics` | A readable analytics summary: post performance, follower growth, daily metrics |
| `/postzen-connect` | Guided flow to link a new social account |

## Tools

The MCP server exposes 44 tools; Grok picks them up automatically. The main groups:

| Group | Tools |
|---|---|
| Posts | `createPost`, `listPosts`, `getPost`, `updatePost`, `deletePost`, `createMediaPresign` |
| Queues | `listQueueSlots`, `createQueueSlot`, `updateQueueSlot`, `deleteQueueSlot`, `getNextQueueSlot`, `previewQueue` |
| Accounts | `listAccounts`, `createConnectUrl`, `completeConnect`, `disconnectAccount`, Pinterest board tools |
| Analytics | `getAnalytics`, `getPostTimeline`, `getDailyMetrics`, `getFollowerStats`, `getBestTimeToPost`, `syncExternalPosts` |
| Profiles, API keys, webhooks | `listProfiles`, `createProfile`, `listApiKeys`, `createWebhook`, `listWebhookDeliveries`, and more |

## Example prompts

```text
Post this launch announcement to X and LinkedIn tomorrow at 9am PT
```

```text
Take this blog post and turn it into a thread for X and a LinkedIn post, save both as drafts
```

```text
How did my posts perform this week?
```

```text
Add these three posts to my queue
```

```text
Connect my Pinterest account
```

## Safety

Publishing is an outward-facing action: the skills always show you the final per-platform content and timing and ask for confirmation before anything goes live. Drafts never require confirmation. The skills never ask for social media passwords or tokens — account connections always go through the platform's own browser-based flow.

## Authentication, network access, and data

- The plugin connects only to PostZen's hosted MCP endpoint at `https://mcp.postzen.dev/mcp`, which proxies tool calls to the PostZen API at `https://api.postzen.dev`.
- Authentication is handled through the MCP OAuth flow (sign in to your PostZen account in the browser). The access token carries a PostZen API key scoped to the profiles you approved; you can revoke it at any time in the PostZen dashboard.
- Media uploads go to the presigned upload URL returned by `createMediaPresign` — nothing else on your machine is read.
- No hooks, no shell commands, no local code execution: the plugin is skills plus a remote MCP server.

## Development

```bash
git clone https://github.com/postzen-dev/postzen-grok-plugin
grok --plugin-dir ./postzen-grok-plugin
```

## Links

- [PostZen](https://www.postzen.dev) · [Dashboard](https://app.postzen.dev) · [API docs](https://docs.postzen.dev) · [Status](https://status.postzen.dev)
- Prefer a raw API? See the [agent quickstart](https://www.postzen.dev/agent-quickstart.md), the [Node SDK](https://www.npmjs.com/package/@postzen/node), [Python SDK](https://pypi.org/project/postzen-sdk/), or [CLI](https://www.npmjs.com/package/@postzen/cli).
- Using Claude Code instead? See the [PostZen Claude Code plugin](https://github.com/postzen-dev/postzen-claude-plugin).

## License

MIT
