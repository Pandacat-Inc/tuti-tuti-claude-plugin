# Tuti Tuti for Claude Code

A Claude Code plugin that teaches Claude to write, check and save [Tuti Tuti](https://tutituti.ca) tutorial scripts, and connects the Tuti Tuti MCP servers.

Tuti Tuti is a Chrome extension that performs a narrated tutorial on a live web app from plain sentences. Claude writes the sentences; the extension performs them.

## Install

```
/plugin marketplace add Pandacat-Inc/tuti-tuti-claude-plugin
/plugin install tuti-tuti@pandacat
```

Then ask: "Write the onboarding tour for these three screens with these demo values, and check it."

## What it brings

- **A skill**, `tuti-tuti-script`: the script guide and a complete example, so Claude writes in the format the parser expects and checks every draft before handing it over.
- **The public MCP server** at `https://tutituti.ca/api/mcp`: the script guide, the app guide, the example script, a parser and a checker. No account.
- **The account MCP server** at `https://tutituti.ca/api/mcp/account`: your tutorials, their rework files, saving scripts into your library, and your usage. Run `/mcp` once to sign in. Part of Tuti Tuti Pro.

Claude never plays, rehearses, records or exports a tutorial. That happens in the extension, on your computer.

Full guide: https://tutituti.ca/claude-connector · Questions: support@pandacat.ca

Tuti Tuti - powered by Pandacat.
