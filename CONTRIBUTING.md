# Adding an item

An item enters by a pull request that adds one file, `items/<name>.yaml`, and the maintainer reviews
it. Being listed is what tells Atlas users an item was read by someone, so these rules keep that true:

1. **Reviewed, then pinned.** `sha` is a whole commit, 40 characters. Atlas installs exactly that
   commit. A new version is a new pull request moving `sha`, and the reviewer reads what changed
   between the two commits.
2. **No code that replaces itself.** An item never fetches and runs code from outside the
   reviewed commit: no self-updater, no script downloaded at run time, no screen loading code from
   an address.
3. **Source only.** No compiled files (executables, libraries, Python bytecode, WebAssembly): a
   reviewer cannot read them. A program the item needs is named, and the user installs it.
4. **The smallest kind.** A skill alone is a `skill`; one MCP server is an `mcp` item; a plugin
   that registers an executor is an `executor`; anything bringing more is a `plugin`. The check
   refuses an item whose code is not the kind it says.
5. **Secrets through Atlas.** An MCP server reads each secret as `${NAME}` and declares it; a plugin
   declares its secrets in its manifest. A skill carries none: the commands the model runs never
   see the vault. Nothing reads keys from files, environment variables or other programs' logins.
6. **What it does is said.** What it sends over the network, runs in the background, or reads and
   writes outside its own folder is written in its README.
7. **Pinned servers.** An MCP server runs `npx` with `name@1.2.3`, `uvx` with `name==1.2.3`,
   `docker run` with an image by its digest, or is a remote `https` address.
8. **Pictures from the reviewed tree.** `image` and `screenshots` are files of the item's own
   repository at its `sha`.
9. **Yours to submit.** The pull request comes from the code's owner or a major contributor to it.
   `author` is that GitHub account; `license` is the code's license.
10. **Official is the maintainer's word.** Only the maintainer marks an item `tier: official`, for
    the Atlas team's own.

Check the file before opening the pull request, as the CI does:

```bash
atlas marketplace check items/your-item.yaml          # the file, and its code at its commit
atlas marketplace check items/your-item.yaml --load   # and a plugin's Python loaded, in a home thrown away
```

## Retiring and delisting

An item found unsafe is **retired**: it goes into `removed.yaml` with the reason, and every Atlas that
installed it stops it (a plugin is taken away, its data kept; a skill is archived; an MCP server
leaves `mcp.json`). A retired repository retires everything that came from it. An item that only no
longer fits (unmaintained, superseded) is **delisted**: its file is deleted, and those who installed
it keep it.
