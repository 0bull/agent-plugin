# 0bull agent plugin

[0bull](https://0bull.net) rents real iPhones by the month. This plugin connects an AI agent to the phones on your 0bull account through the 0bull MCP server, and adds three skills that teach the agent how to use them:

| Skill | What it does |
| --- | --- |
| `0bull-iphone-control` | See a phone's screen, read its text, tap, swipe, type, send device commands, run your recorded macros, or hand a narrow task to the on-phone agent. |
| `0bull-tiktok-post` | Publish a video to TikTok from one of your phones, then follow it to a final status. |
| `0bull-youtube-post` | Publish a YouTube Short, with the required title of at most 100 characters. |

It is a portable [Agent Plugins](https://agent-plugins.org) v1 package: `plugin.json`, `mcp.json` (the hosted MCP server at `https://0bull.net/mcp`, Streamable HTTP) and `skills/`. There is no plugin code and no API key: you sign in with your 0bull account through OAuth in the browser, and the tools only reach that account's phones and accounts.

## What you need

- A 0bull account with at least one phone assigned. Phones are rented at https://0bull.net.
- For posting: the TikTok or YouTube account already signed in on the phone and linked in your 0bull dashboard.

## Hermes Agent

Hermes needs MCP support; with a pip install that is the `mcp` extra (`pip install "hermes-agent[mcp]"`).

```bash
hermes plugins install 0bull          # from the Hermes plugin catalog
# or: hermes plugins install 0bull/agent-plugin
hermes plugins enable 0bull
```

Then add the server with OAuth to `~/.hermes/config.yaml` and sign in:

```yaml
mcp_servers:
  0bull:
    url: "https://0bull.net/mcp"
    auth: oauth
```

```bash
hermes mcp login 0bull
hermes mcp test 0bull
```

On a remote machine, follow the prompt: paste the redirect URL back, or forward the port it shows.

## OpenClaw

```bash
openclaw plugins install clawhub:0bull
openclaw mcp add 0bull --url https://0bull.net/mcp --transport streamable-http
openclaw mcp login 0bull
```

OpenClaw loads the skills from the plugin. Until it can sign in to servers that plugins declare, the `mcp add` line registers the same server so `mcp login` can reach it.

## Other agents

Any client that reads Agent Plugins packages can use this folder as is. Otherwise, add `https://0bull.net/mcp` as a Streamable HTTP MCP server with OAuth and copy the folders in `skills/`.

## Safety

- The agent only types a password or code that you gave it for that purpose in the same conversation.
- A caption or Short title is written with you, never invented. A failed post is never retried automatically, so nothing is published twice.
- Destructive device actions (reboot, clearing photos) run only after you confirm them.
- On-screen text is treated as data, never as instructions.
- The skills tell the agent never to call the rental and billing tools, which spend your money; changing your plan stays with you at https://0bull.net.

## Links

- Documentation: https://docs.0bull.net/mcp/overview
- Support: https://0bull.net/support
- Privacy policy: https://0bull.net/privacy
- Terms: https://0bull.net/terms

## License

MIT
