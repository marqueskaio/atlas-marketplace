# Atlas marketplace

What people made for [Atlas](https://github.com/marqueskaio/atlas): skills, MCP servers, plugins and
executors. Every item entered by a pull request the maintainer reviewed, and installs exactly as it
was reviewed: its code is pinned to a whole commit, and a new version is a new pull request.

## Using it

In the Atlas app, open **Marketplace** in the sidebar: a tab per kind, the Atlas team's items first,
a page per item with its README and what it needs on your machine. In a terminal:

```bash
atlas marketplace list
atlas marketplace show pdf-tools
atlas marketplace install pdf-tools
atlas marketplace update pdf-tools
atlas marketplace remove pdf-tools
```

A plugin arrives off: turning it on shows what it declares, for your consent. A skill that needs an
MCP server brings it along.

## The four kinds

| Kind | What it is | Where it goes |
|---|---|---|
| `skill` | One folder with a `SKILL.md` | The home's skills |
| `mcp` | One MCP server: its `mcp.json` entry | The home's `mcp.json`, its secrets in the vault |
| `plugin` | Anything that brings more: skills with a server of its own, hooks, a screen, Python | The home's plugins, off |
| `executor` | A plugin that registers an executor | The home's plugins, off |

## Making an item

Anyone with a GitHub account can submit one. The code stays in your own repository; the pull
request here only adds the file that points at it.

- **A plugin or an executor** is written with Atlas's plugin SDK. `atlas plugins new my-plugin`
  starts the smallest one that works (a manifest, a tool and a skill), and
  `atlas plugins validate <folder>` loads it as Atlas does and says what is wrong. Its Python
  imports only `atlas.plugins.sdk` and its screen only `@atlas/plugin-sdk`, both of which come with
  Atlas. The whole guide is
  [docs/PLUGINS.md](https://github.com/marqueskaio/atlas/blob/main/docs/PLUGINS.md).
- **A skill** needs no SDK: a folder with a `SKILL.md`, which says under `metadata.requires` the
  programs, MCP servers and systems it needs.
- **An MCP server** is any one published on npm, PyPI or a container registry, at an exact version.

Atlas can write it with you: type `/write-plugin` in a conversation, or press "Make your own" in
the app's Marketplace. Then check your item's file as the CI will, with
`atlas marketplace check items/<name>.yaml --load`.

## An item's file

`items/<name>.yaml`:

```yaml
name: pdf-tools
kind: skill
title: PDF tools
summary: Read, fill and merge PDF files.
author: your-github-account
license: MIT
repo: your-github-account/atlas-skills
sha: 0123456789abcdef0123456789abcdef01234567
path: pdf-tools
version: "1.0.0"
```

An MCP server holds its entry instead of a repository:

```yaml
name: docs
kind: mcp
title: Docs
summary: Search the documentation of the libraries you use.
author: your-github-account
license: MIT
homepage: https://example.com/docs-mcp
server:
  command: npx
  args: ["-y", "docs-mcp@1.2.3"]
  env:
    DOCS_KEY: "${DOCS_KEY}"
secrets:
  - name: DOCS_KEY
    description: Your docs API key.
    url: https://example.com/keys
```

The whole guide, with what a skill says it needs, is in Atlas's
[plugin guide](https://github.com/marqueskaio/atlas/blob/main/docs/PLUGINS.md#publishing-it-in-the-marketplace).
What an item must be to enter is in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This repository's own files are MIT. Each item keeps the license its own repository gives it.
