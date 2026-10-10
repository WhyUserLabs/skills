# WhyUser skills

Skills that let an AI assistant work with [WhyUser](https://whyuser.com).

| Skill | What it does |
|---|---|
| `whyuser-workflows` | Runs a WhyUser workflow end to end, such as **Content Revise**: revise a page, blog post or case study against what a simulated buying committee found, then save it for WhyUser to audit. |

The steps of each workflow come from WhyUser's server when the workflow runs, so the skill itself rarely changes. It holds no keys and no customer data.

## 1. Connect WhyUser

In WhyUser, open **Settings → AI assistant access → New key**. The panel shows the server address and a configuration to paste for Claude Code, Claude Desktop, Cursor, VS Code, Codex and Zed. Use one key per client.

## 2. Install the skill

**Claude Code**

```
/plugin marketplace add WhyUserLabs/skills
/plugin install whyuser@whyuser
```

**Claude (web and desktop app)**

1. Download [`dist/whyuser-workflows.zip`](dist/whyuser-workflows.zip).
2. In Claude, open **Customize → Skills**, click **+**, then **Create skill → Upload a skill**, and choose the zip.

Code execution must be on. Team and Enterprise owners can add the skill for everyone under **Organization settings → Plugins & skills**.

**Cursor, Codex, VS Code, Zed and other MCP clients**

No skill is needed. Ask for the workflow by name and the assistant reads its steps from WhyUser.

## 3. Use it

> Revise https://example.com/pricing with WhyUser and get it ready for sign-off.

The assistant asks one question (which simulation to work from), writes the revised content, and saves it. WhyUser audits the result and returns a link to the run.

## What is in this repository

```
.claude-plugin/marketplace.json                      the marketplace Claude Code adds
plugins/whyuser/.claude-plugin/plugin.json           the plugin
plugins/whyuser/skills/whyuser-workflows/SKILL.md    the skill
dist/whyuser-workflows.zip                           the same skill, packaged for upload to Claude
```

## Changing the skill

1. Edit `plugins/whyuser/skills/whyuser-workflows/SKILL.md`.
2. Rebuild the zip from the repository root:

   ```
   rm -f dist/whyuser-workflows.zip
   (cd plugins/whyuser/skills && zip -r -X ../../../dist/whyuser-workflows.zip whyuser-workflows)
   ```

3. Raise `version` in `plugins/whyuser/.claude-plugin/plugin.json`. Claude Code users stay on the version they installed until it changes.
4. Check it with `claude plugin validate .`

## License

[Apache License 2.0](LICENSE). The WhyUser name and logo are not covered by this license.
