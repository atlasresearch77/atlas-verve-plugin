# Atlas Verve plugins for Claude

**Atlas Connect:** draft from your AI assistant, approve in Atlas Verve.

This repository is a [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces.md) with one
plugin, `atlas-verve`. It connects Claude to your Atlas Verve AI workspace so Claude can draft LinkedIn posts in your own
voice, revise them and send them to you for review. **Claude can never approve, schedule or publish a post.** You do that
yourself in Atlas Verve after signing in.

## Before you start

- You need an Atlas Verve AI account (`https://atlasverve.com`).
- Atlas Connect must be available for the workspace (an administrator controls this) and you must turn it on for your
  own account in Atlas Verve **Settings**. It is off by default.
- Read what it can and cannot do: <https://atlasverve.com/atlas-connect>.

## Install in Claude Code

```
claude plugin marketplace add atlasresearch77/atlas-verve-plugin
claude plugin install atlas-verve@atlas-verve-plugins
```

Then start `claude` (or run `/reload-plugins`), run `/mcp`, choose **plugin:atlas-verve:atlas-verve** and **Authenticate**. Sign in with your usual Atlas Verve login and
approve the permissions on the consent screen. Check that the account shown is yours before you approve.

Try: *"Get my voiceprint from atlas-verve"*, then *"Create a draft about why human review matters for AI-written posts"*.
The plugin also adds the skill `/atlas-verve:draft-linkedin-post`, which guides Claude through the whole flow.

## What to expect

1. Claude reads your voice, then starts the draft and tells you the post id straight away.
2. Writing takes about 20 to 40 seconds. Claude then shows the title, the text and a summary of claims to check.
3. Ask for changes as often as you like. Each revision is a new version.
4. When you are happy, Claude gives you a review link. Open it, sign in and approve it yourself. Until then the post stays a
   draft, and asking Claude to "just publish it" will be refused.

## Update or remove

```
claude plugin marketplace update atlas-verve-plugins
claude plugin update atlas-verve@atlas-verve-plugins
claude plugin uninstall atlas-verve@atlas-verve-plugins
```

In the Claude desktop app, open **Plugins** and choose **Connect** to sign in.

If you previously ran `claude mcp add ... atlas-verve` by hand, remove it (`claude mcp remove atlas-verve`) so you do not have
the connector twice.

## Other ways to connect

| App | Where | Steps |
|---|---|---|
| Claude Code | terminal | the two install commands above, then `/mcp` |
| Claude Desktop | Customize, Plugins, Atlas verve | install the plugin, click **Connect** |
| Claude Connectors | Settings, Connectors | **Add custom connector** with the URL below |
| Other MCP apps | app settings | the URL below (streamable HTTP, OAuth) |

The same steps, as tabs, are at <https://atlasverve.com/atlas-connect>. ChatGPT has not been tested yet.

- **Claude on the web, desktop or mobile:** add a custom connector with the URL `https://mcp.atlasverve.com/mcp`
  (Settings, Connectors, Add custom connector), if your plan allows custom connectors.
- **Any MCP client:** the server is at `https://mcp.atlasverve.com/mcp` (streamable HTTP, OAuth).

## What this repository contains

```
.claude-plugin/marketplace.json            the marketplace catalog
plugins/atlas-verve/.claude-plugin/plugin.json
plugins/atlas-verve/.mcp.json              the connector (remote server URL only)
plugins/atlas-verve/skills/draft-linkedin-post/SKILL.md
```

There is no code and there are no secrets here. Sign-in happens in your browser with Atlas Verve; this plugin never sees
your password or tokens.

## Support

Use the contact page: <https://atlasverve.com/contact>.

## Licence

MIT. See [LICENSE](LICENSE). This covers these plugin files only, not the Atlas Verve service.
