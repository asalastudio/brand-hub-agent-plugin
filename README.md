# Asala Brand Hub agent plugin

A Claude Code plugin marketplace with one plugin, `asala-brand-hub`. It adds a skill that teaches coding agents how to
work with a client's brand from the hub: read the brand kit first, keep `BRAND.md` current, use approved hub files,
file new work back into boards, never approve.

It does **not** contain the hub connection. Each client has its own key, so the connection is added per client project
with the command from the hub's **Open in → Claude Code** dialog (local scope: the key stays out of the repo and away
from other clients' projects).

## Publish

1. Push this folder as the root of its own GitHub repository, for example `asalastudio/brand-hub-agent-plugin`.
2. Set `VITE_AGENT_PLUGIN_REPO=asalastudio/brand-hub-agent-plugin` in the hub's environment and redeploy. The
   "Add the brand skill" step then appears in **Open in → Claude Code**.

## Install (what users run)

```sh
claude plugin marketplace add asalastudio/brand-hub-agent-plugin
claude plugin install asala-brand-hub@asala
```

## Check before publishing

```sh
claude plugin validate agent-plugin
claude plugin validate agent-plugin/plugins/asala-brand-hub
```
