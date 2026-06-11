# qwen-marketplace-demo

A minimal **Qwen Code** extension marketplace for testing marketplace source
parsing, discovery, and install.

The marketplace manifest lives at the repo root in
[`qwen-marketplace.json`](./qwen-marketplace.json) and lists two self-contained
example extensions under [`extensions/`](./extensions):

| Entry          | Format         | What it tests                                   |
| -------------- | -------------- | ----------------------------------------------- |
| `hello-qwen`   | Qwen native    | A `qwen-extension.json` extension, no conversion |
| `hello-claude` | Claude plugin  | Auto-conversion of a `.claude-plugin/plugin.json` on install |

Both entries use relative `source` paths resolved inside this repo (against
`metadata.extensionRoot: "extensions"`), so the whole add → discover → install
flow works without any external dependencies.

## Try it

```bash
# Add this repo as a marketplace source
qwen extensions sources add BZ-D/qwen-marketplace-demo

# List configured sources (shows format = qwen)
qwen extensions sources list

# Browse / install a specific entry
qwen extensions install BZ-D/qwen-marketplace-demo:hello-qwen
qwen extensions install BZ-D/qwen-marketplace-demo:hello-claude
```

Or open the interactive manager with `/extensions manage` and use the
**Sources** / **Discover** tabs.

## Layout

```
qwen-marketplace.json
extensions/
  hello-qwen/
    qwen-extension.json
    commands/hello.md
  hello-claude/
    .claude-plugin/plugin.json
    commands/greet.md
```
