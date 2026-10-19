# Troubleshooting

Check the Obsidian plugin and OpenAI tunnel-client documentation first because endpoint behavior and UI steps may change. Never share an API key while asking for help.

| Symptom | Checks and next steps |
| --- | --- |
| ChatGPT cannot find the tunnel | Confirm the tunnel and ChatGPT connection use the same OpenAI organization/workspace, the account has the required access, and the current tunnel-client process is running. Review the official tunnel guide. |
| No Obsidian tools appear | Confirm the plugin is enabled, MCP is enabled in its settings, the endpoint is correct, and the tunnel forwards to the MCP endpoint rather than a REST-only route. Reconnect or refresh the server tools in ChatGPT. |
| Local connection refused | Make sure Obsidian is open, the plugin is enabled, the port and URL match plugin settings, and local security software is not blocking loopback traffic. |
| `401 Unauthorized` | Check that the Obsidian `Authorization` header is `Bearer <plugin key>` with no extra quotes or whitespace. Confirm it is not accidentally using the OpenAI tunnel runtime key. Rotate the plugin key if it may have been exposed. |
| TLS or certificate error | Follow the plugin's certificate-trust instructions for the current operating system. Confirm the client connects to the configured hostname and port. Avoid disabling TLS verification. |
| HTTP 400 during a hand-written test | A raw POST to a guessed discovery route or an incorrectly typed JSON body may not be a valid MCP test. Use the protocol discovery flow of the client and the plugin's documented MCP endpoint. Confirm the content type and protocol version expected by the current implementation. |
| OAuth metadata returns 401 | This can mean the client attempted OAuth discovery against a server that uses a bearer key instead. Configure the authentication method supported by the plugin and tunnel; do not assume every MCP server uses OAuth. |
| Works on the same computer but not in hosted ChatGPT | The loopback address belongs to the local computer. Use Secure MCP Tunnel; do not expect ChatGPT's hosted service to reach `127.0.0.1`. |
| Tunnel connects but tool calls fail | Check the tunnel-client logs locally, the endpoint's certificate/auth configuration, and whether the local plugin remains available. Redact credentials before sharing logs. |

## Safe diagnostic information to share

You can share your operating system, Obsidian/plugin/tunnel-client version, the non-secret error text, and whether the failure occurs during tunnel discovery or tool execution. Remove bearer values, API keys, runtime keys, private URLs, vault names, note contents, and personal paths from logs and screenshots.

## Official sources

- [Plugin documentation](https://github.com/coddingtonbear/obsidian-local-rest-api)
- [OpenAI tunnel troubleshooting](https://github.com/openai/tunnel-client/blob/master/docs/troubleshooting.md)
- [Secure MCP Tunnels](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels)
- [ChatGPT custom MCP setup](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)

