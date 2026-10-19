# Recommended GitHub repository settings

Use these settings when publishing this guide or a derivative:

- **Visibility:** Public, if the intent is to share with the community.
- **Repository name:** `obsidian-chatgpt-mcp-guide` (or another clear name).
- **Description:** `A practical, security-first guide to connect an Obsidian vault to ChatGPT with MCP and Secure MCP Tunnel.`
- **Default branch:** `main`.
- **Topics:** `obsidian`, `chatgpt`, `mcp`, `secure-mcp-tunnel`, `documentation`.
- **Issues:** On, with a template reminding reporters never to post secrets.
- **Discussions:** Optional; enable only if maintainers plan to moderate questions.
- **Wiki / Pages:** Off unless there is a specific maintenance plan. The README is the canonical guide.
- **Branch protection:** Protect `main` from force pushes and deletion. For a solo-maintained guide, requiring a pull request may be optional; enable it when there are multiple maintainers.
- **Secrets and variables:** Do not add personal API keys as repository secrets. This documentation repository does not need credentials or CI secrets.
- **Security features:** Keep secret scanning and push protection enabled when available for the account/repository.

## Initial repository checklist

1. Create the repository with the selected visibility and a short description.
2. Add the files in this repository, including the license and security policy.
3. Ensure the default branch is `main` and no generated local configuration or `.env` file was uploaded.
4. Add the suggested topics and enable Issues if accepting reports.
5. Review the published README and guide links while signed out or in a private browser window.

## Settings applied to this repository

The published repository is configured as follows:

- Public visibility, `main` as the default branch, and the description above.
- Topics: `obsidian`, `chatgpt`, `mcp`, `secure-mcp-tunnel`, and `documentation`.
- Issues and pull requests enabled; Discussions and Wikis disabled.
- A `protect-main` ruleset blocks force pushes and branch deletion on `main`. It does not require pull requests, so the solo maintainer can continue making direct updates.
- GitHub secret scanning and push protection are enabled, along with private vulnerability reporting.
- GitHub Pages is not configured; the README remains the canonical guide.

GitHub settings and feature names vary by plan and over time. Use the current repository **Settings** pages and GitHub documentation.

