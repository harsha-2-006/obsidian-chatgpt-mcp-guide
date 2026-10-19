# Connect Obsidian to ChatGPT

This guide connects an Obsidian vault on your computer to ChatGPT's hosted service through MCP (Model Context Protocol). The intended route is the Obsidian plugin's local MCP endpoint plus OpenAI Secure MCP Tunnel.

## How the pieces fit

The Obsidian plugin provides an authenticated endpoint on the same computer as the vault. A local `tunnel-client` process makes an outbound connection to OpenAI and forwards MCP traffic. ChatGPT connects through that tunnel. ChatGPT cannot directly access your computer's `127.0.0.1` address.

```text
ChatGPT ── Secure MCP Tunnel ──> tunnel-client ──> https://127.0.0.1:27124/mcp/ ──> Obsidian vault
```

The plugin's published default endpoint is `https://127.0.0.1:27124/mcp/`. Confirm the URL and authentication details in the plugin's current settings and documentation before configuring a client.

## 1. Install and configure the Obsidian plugin

1. Open Obsidian Desktop and select the vault you intend to connect.
2. Open **Settings → Community plugins → Browse**.
3. Find **Local REST API with MCP** by `coddingtonbear`, install it, and enable it.
4. Open the plugin settings. Confirm that MCP support is enabled and note the endpoint shown there.
5. Create or copy the plugin's API key. Store it in a password manager or a protected secret store. Do not put it in a shared configuration file.
6. Keep Obsidian open while using the connection.

The plugin creates a local certificate authority for HTTPS. Follow the plugin's current instructions to trust its certificate where required. Do not turn off certificate checks as a general fix. If you choose the plugin's optional HTTP mode, use it only on the local machine and ensure the tunnel forwards to the correct local endpoint; HTTPS is preferred.

## 2. Create an OpenAI Secure MCP Tunnel

Follow OpenAI's current [Secure MCP Tunnel guide](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels) for account setup, tunnel creation, and installing the current `tunnel-client` release.

1. In OpenAI Platform, create a tunnel and obtain its tunnel ID and runtime credentials as described in the official guide.
2. Install the current tunnel-client release on the computer that runs Obsidian. Use the official download and quickstart for your operating system; commands and packaging can change between releases.
3. Configure the client to forward to the Obsidian MCP URL and supply the Obsidian authorization header from a secret reference.
4. Keep the client process running while ChatGPT needs access. It uses an outbound connection; do not open an inbound router port.

### Configuration shape (placeholders only)

Use the exact schema documented for your installed tunnel-client version. This illustrates the separation of the tunnel credential and Obsidian credential; it is not a ready-to-run file:

```yaml
config_version: 1
control_plane:
  base_url: https://api.openai.com
  tunnel_id: tunnel_<your-tunnel-id>
  api_key: env:CONTROL_PLANE_API_KEY
mcp:
  server_urls:
    - channel: main
      url: https://127.0.0.1:27124/mcp/
  extra_headers:
    Authorization: env:OBSIDIAN_AUTHORIZATION
```

Set `CONTROL_PLANE_API_KEY` to the tunnel runtime credential and `OBSIDIAN_AUTHORIZATION` to the complete header value `Bearer <your-Obsidian-plugin-key>`. Do not commit these values. Some tunnel-client versions may require a different config shape or certificate setup; follow the versioned official reference. Environment-variable references avoid embedding values in the YAML, but protect the environment and any secret files too.

For an HTTP MCP endpoint, OpenAI's guide uses the tunnel-client HTTP server URL option; do not substitute a shell command transport. Consult the current quickstart and configuration reference for exact flags.

## 3. Add the tunnel to ChatGPT

Use OpenAI's current [custom MCP server instructions](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt). The interface and availability can vary by account, plan, and workspace policy.

1. Open ChatGPT's connector/app settings and choose the option to add a custom MCP server.
2. Choose the **Tunnel** connection type and select the tunnel created above. Associate the correct workspace/organization as requested.
3. Configure authentication as directed so the Obsidian `Authorization: Bearer …` header reaches the local MCP server. Keep the OpenAI tunnel runtime key separate; it authenticates the tunnel client, not the Obsidian plugin.
4. Review the server and requested tool permissions before confirming. If the tunnel is not listed, check the Platform organization, workspace, permissions, and that `tunnel-client` is running.
5. Open the available tools in ChatGPT and confirm that the expected Obsidian tools appear.

If ChatGPT shows a direct URL option instead, do not enter `127.0.0.1` expecting the hosted service to reach your computer. Use the tunnel route for a local vault, or follow a currently supported public HTTPS deployment design with appropriate authentication and network controls.

## 4. Verify safely

1. Start with a read-only tool such as listing files at the vault root. Check that returned filenames are expected and do not include anything you intended to exclude.
2. Read a harmless test note and confirm the contents are correct.
3. Keep writes disabled or unused until you need them. If you later use a write tool, make a vault backup, create a disposable test note, and verify the exact change before using it on important notes.
4. Stop the tunnel-client process when finished if you do not want the connection available continuously.

## Local MCP clients

A local MCP client running on the same computer may connect directly to the plugin's loopback endpoint using the plugin's documented authentication. That is a separate setup from hosted ChatGPT. The local client must trust the plugin certificate and send the bearer token securely. Never assume a remote or hosted client can reach a loopback address on your computer.

## Disconnect or rotate credentials

Remove the custom MCP connection from ChatGPT and revoke/rotate the plugin key in the plugin settings if you no longer want access. Revoke or rotate the OpenAI tunnel runtime credential separately. Stop the client process and remove old secret references. Follow current provider instructions for credential revocation.

## References

- [Obsidian plugin setup and endpoint](https://github.com/coddingtonbear/obsidian-local-rest-api)
- [ChatGPT custom MCP servers](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
- [OpenAI Secure MCP Tunnels](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels)
- [tunnel-client configuration](https://github.com/openai/tunnel-client/blob/master/docs/configuration.md)

