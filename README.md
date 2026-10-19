# Connect Obsidian to ChatGPT with MCP

A practical, security-first guide to connect an Obsidian vault to ChatGPT using the **Local REST API with MCP** community plugin and OpenAI's **Secure MCP Tunnel**. It is written for people who want ChatGPT to work with notes stored on their own computer.

## Project origin

This is an independent community contribution about using ChatGPT with Obsidian. ChatGPT's **Computer Use** option was used to create and configure this GitHub repository. ChatGPT also helped draft the documentation, and the files were committed through the connected GitHub integration. Obsidian is the subject of the guide, not a co-author. This repository is not an official OpenAI or Obsidian project and neither organization endorses it.

> **At a glance:** the Obsidian plugin exposes an authenticated MCP endpoint on your computer. ChatGPT's hosted service cannot reach `localhost` directly, so use the Secure MCP Tunnel to carry the connection from your computer to ChatGPT. Do not expose the plugin port to the public internet.

## Before you start

- Obsidian Desktop with a vault you control.
- The community plugin **Local REST API with MCP** by coddingtonbear.
- A ChatGPT account/workspace and plan that currently supports custom MCP connections and Secure MCP Tunnels. Availability and UI labels can change; check the official OpenAI documentation linked below.
- OpenAI Platform access to create/use a tunnel and the current `tunnel-client` application.
- Permission to install software and keep the tunnel client running on the computer that has the vault.

## Setup

Follow [the complete setup guide](docs/connect-obsidian-to-chatgpt.md). It covers the Obsidian plugin, tunnel configuration, ChatGPT connection, safe verification, and cleanup. If something fails, see [troubleshooting](docs/troubleshooting.md).

### Connection shape

```text
ChatGPT hosted service
        │ Secure MCP Tunnel
        ▼
tunnel-client on your computer ── HTTPS ──> Obsidian Local REST API with MCP
                                               │
                                               ▼
                                         Your local vault
```

The computer running Obsidian and `tunnel-client` must remain on and connected while ChatGPT uses the tools. The tunnel does not make the vault available when that computer is offline.

## Start read-only

After connecting, inspect the tool list and try a root file listing or read a harmless test note. Only enable or invoke write-capable tools when you understand the exact effect. Back up your vault before allowing edits, and test with a disposable note first.

## Security essentials

- Treat the Obsidian API key and OpenAI tunnel runtime key as separate secrets.
- Never paste real keys into a repository, issue, screenshot, terminal transcript, or shared chat.
- Keep secrets in environment variables or a secret file with restrictive permissions; examples in this repository use placeholders only.
- Do not configure router port forwarding, public reverse proxies, or firewall rules that expose the Obsidian endpoint.
- Review requested tools and their permissions. A connected MCP server may be able to read or change vault content depending on the tools enabled and the user's instruction.
- Rotate/revoke credentials if they are exposed. See [Security](SECURITY.md).

## Contents

- [Connection guide](docs/connect-obsidian-to-chatgpt.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Recommended repository settings](REPOSITORY_SETTINGS.md)
- [Contributing](CONTRIBUTING.md)
- [Security reporting](SECURITY.md)
- [License](LICENSE)

## Official references

- [Obsidian Local REST API with MCP](https://github.com/coddingtonbear/obsidian-local-rest-api)
- [Obsidian Community Plugins listing](https://community.obsidian.md/plugins/obsidian-local-rest-api/)
- [ChatGPT: connecting custom MCP servers](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
- [OpenAI Secure MCP Tunnels](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels)
- [OpenAI MCP connector guide](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)
- [OpenAI tunnel-client](https://github.com/openai/tunnel-client)
- [tunnel-client configuration reference](https://github.com/openai/tunnel-client/blob/master/docs/configuration.md)
- [tunnel-client troubleshooting](https://github.com/openai/tunnel-client/blob/master/docs/troubleshooting.md)

These products change over time. Prefer the official references for current eligibility, UI labels, downloads, and configuration options. This community guide is independent and is not endorsed by OpenAI, Obsidian, or the plugin author.

