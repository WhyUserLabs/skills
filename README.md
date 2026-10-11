# WhyUser skills

Skills that let an AI assistant work with [WhyUser](https://whyuser.com).

| Skill | What it does |
|---|---|
| `whyuser-workflows` | Runs a WhyUser workflow end to end, such as **Content Revise**: revise a page, blog post or case study against what a simulated buying committee found, then save it for WhyUser to audit. |

The steps of each workflow come from WhyUser's server when the workflow runs, so the skill itself rarely changes. This repository holds no keys and no customer data.

## 1. Install

The skill only tells the assistant how to run a workflow. The WhyUser connection (the MCP server and your key) is what gives it your data and the workflow's steps. **Every assistant needs the connection; without it the skill does nothing.** Claude also uses the skill.

Create a key in WhyUser under **Settings → AI assistant access → New key**: one key per client. It starts with `wu-mcp-` and is shown once.

**Claude Code** (one plugin does both)

```
/plugin marketplace add WhyUserLabs/skills
/plugin install whyuser@whyuser
```

Claude Code asks for your WhyUser MCP key and keeps it in your system's secure storage. To enter it again later, for example after you replace the key, run `/plugin configure whyuser@whyuser`. The plugin connects to `https://mcp.whyuser.com/mcp`. If you already added WhyUser with `claude mcp add`, remove that entry (`claude mcp remove whyuser`) so the tools are not listed twice.

**Claude desktop app** (two steps)

1. Connect WhyUser. On the new key in WhyUser, open **Show client configuration**, copy **Claude Desktop** into `claude_desktop_config.json`, and restart Claude. It connects through the `mcp-remote` bridge, which needs Node. Skip this step if WhyUser is already a connector on your Claude account (below): connectors work in the desktop app too.
2. Add the skill. Download [`dist/whyuser-workflows.zip`](dist/whyuser-workflows.zip). In Claude, open **Customize → Skills**, click **+**, then **Create skill → Upload a skill**, and choose the zip. Code execution must be on.

**Claude on the web (and the desktop app, through your account)**

Add WhyUser as a custom connector with the server URL `https://mcp.whyuser.com/mcp`, **Authentication: No sign-in**, and the request header `authorization` set to `Bearer wu-mcp-…`. On Free, Pro and Max you add it yourself under **Customize → Connectors**, with your own key. On Team and Enterprise an owner adds it under **Organization settings → Connectors**, and everyone shares that one key and its access level, so a read-only key is the safe choice. Claude's request headers are a beta that not every account has yet: without them, use the desktop app's config file or Claude Code. Then add the skill as in step 2 above, or an owner adds it for everyone under **Organization settings → Plugins & skills**.

**Cursor, Codex, VS Code, Zed and other MCP clients**

Connect with the configuration WhyUser's Settings shows for your client. No skill is needed: ask for the workflow by name and the assistant reads its steps from WhyUser.

## 2. Use it

> Revise https://example.com/pricing with WhyUser and get it ready for sign-off.

The assistant asks one question (which simulation to work from), writes the revised content, and saves it. WhyUser audits the result and returns a link to the run.

## What is in this repository

```
.claude-plugin/marketplace.json                      the marketplace Claude Code adds
plugins/whyuser/.claude-plugin/plugin.json           the plugin: the WhyUser server and the key prompt
plugins/whyuser/skills/whyuser-workflows/SKILL.md    the skill
dist/whyuser-workflows.zip                           the same skill, packaged for upload to Claude (no server, no key)
```

## Changing the plugin

1. Edit `plugins/whyuser/skills/whyuser-workflows/SKILL.md`.
2. Rebuild the zip from the repository root:

   ```
   rm -f dist/whyuser-workflows.zip
   (cd plugins/whyuser/skills && zip -r -X ../../../dist/whyuser-workflows.zip whyuser-workflows)
   ```

3. Raise `version` in `plugins/whyuser/.claude-plugin/plugin.json`. Claude Code users stay on the version they installed until it changes.

   The server address (`mcpServers`) and the key prompt (`userConfig`) are in the same file. Skip step 2 when only those change: the zip holds the skill alone.

4. Check it with `claude plugin validate .`

## License

[Apache License 2.0](LICENSE). The WhyUser name and logo are not covered by this license.
