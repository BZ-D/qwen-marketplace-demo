# qwen-marketplace-demo

A minimal **Qwen Code** extension marketplace for testing marketplace source
parsing, discovery, and install.

The marketplace manifest lives at the repo root in
[`qwen-marketplace.json`](./qwen-marketplace.json) and lists two self-contained
example extensions under [`extensions/`](./extensions). Each bundles a command,
a skill, and an MCP server so the full install surface is exercised:

| Entry          | Format        | Command  | Skill             | MCP server                          |
| -------------- | ------------- | -------- | ----------------- | ----------------------------------- |
| `hello-qwen`   | Qwen native   | `/hello` | `greeting-helper` | `everything` (stdio, `npx`)         |
| `hello-claude` | Claude plugin | `/greet` | `farewell-helper` | `demo-http` (Claude `http` → `httpUrl`) |

Both entries use relative `source` paths resolved inside this repo (against
`metadata.extensionRoot: "extensions"`), so the whole add → discover → install
flow works without any external dependencies.

`hello-claude` is intentionally authored in **Claude plugin** format
(`.claude-plugin/plugin.json`) to exercise auto-conversion on install:

- its Markdown command is collected like a native command, and
- its MCP server declared as `{ "type": "http", "url": "…" }` is converted to
  Qwen's `{ "httpUrl": "…" }` shape (the `demo-http` URL is a placeholder — it
  demonstrates the conversion, it is not a live endpoint).

## Try it

```bash
# Add this repo as a marketplace source
qwen extensions sources add BZ-D/qwen-marketplace-demo

# List configured sources (shows format = qwen)
qwen extensions sources list

# Install a specific entry
qwen extensions install BZ-D/qwen-marketplace-demo:hello-qwen
qwen extensions install BZ-D/qwen-marketplace-demo:hello-claude
```

Or open the interactive manager with `/extensions manage` and use the
**Sources** / **Discover** tabs (the Discover detail lists the command, skill,
and MCP server each entry will install).

## Layout

```
qwen-marketplace.json
extensions/
  hello-qwen/
    qwen-extension.json          # native, with mcpServers.everything
    commands/hello.md
    skills/greeting-helper/SKILL.md
  hello-claude/
    .claude-plugin/plugin.json   # Claude format, with http MCP server
    commands/greet.md
    skills/farewell-helper/SKILL.md
```
