# TSLink for Claude Code

Open a running local app on your phone or share a private HTTPS preview with
a colleague. This plugin gives Claude Code the `tslink-preview` skill and a
TSLink MCP connection so the agent can share a port, retrieve its exact URL,
and remove the preview when you agree.

## Prerequisites

Install the [TSLink CLI](https://github.com/anydoor7/tslink#installation)
locally and make `tslink` available on Claude Code's `PATH`. The plugin does
not install or bundle the CLI. It requires a local binary and does not work
on surfaces without one, such as claude.ai chat. Private recipients need
Tailscale and permission to reach the app. Fresh nodes can require browser
enrollment and device approval by the tailnet owner.

## Install

In Claude Code, run:

```text
/plugin marketplace add anydoor7/tslink
/plugin install tslink@tslink
```

Reload plugins or restart Claude Code, then ask to open your running app on
your phone or invoke `/tslink:tslink-preview`. Avoid adding a second TSLink
MCP connection in the same client.

## What runs and connects

Claude Code fetches this plugin from GitHub through the
`anydoor7/tslink` marketplace. The plugin launches the locally installed
`tslink mcp` stdio server. Claude Code sends tool arguments to that process
and receives tool results, which can include app configuration, identities,
URLs and requested logs. The skill can also invoke the local TSLink CLI to
check, share, inspect and remove previews, and check that the local app
responds. It asks before installing a missing CLI or sharing the app.

TSLink talks to Tailscale for coordination, enrollment, encrypted peer or
relay transport and HTTPS certificates. Its embedded Tailscale SDK uploads
diagnostic logs to Tailscale by default. App names appear in public
certificate transparency logs. Administrative operations can contact the
Tailscale API. Custom coordination servers and configured alert webhooks or
commands receive data for those features. Sharing may install a persistent
local background service. The preview skill asks about access and that
service before sharing and does not enable Funnel or create guest links.
Other TSLink MCP tools can publish apps when explicitly requested. See the
[integration guide](https://github.com/anydoor7/tslink/blob/main/docs/agent-integrations.md).

## Privacy Policy

The [TSLink Privacy Policy](https://github.com/anydoor7/tslink/blob/main/README.md#privacy-policy)
covers the local CLI, MCP server and this plugin. TSLink processes selected
app traffic, service configuration, identities and access rules, and stores
configuration, node state and logs locally. Optional administrative
credentials use the system credential store with a conditional file fallback
on macOS/Linux. Access history has configurable retention; removing a
preview does not erase historical logs. Tailscale and any configured
services receive the data described above. TSLink has no separate maintainer
analytics collector. Your MCP client and model provider receive tool
arguments and results under their own retention policies. Read the linked
policy for storage locations, retention limits and deletion commands.

## Support and maintenance

Use the [TSLink issue tracker](https://github.com/anydoor7/tslink/issues)
for support. Do not post credentials, private links, logs or personal data
in public issues. Follow [SECURITY.md](https://github.com/anydoor7/tslink/blob/main/SECURITY.md)
for sensitive security reports.

The canonical skill lives in
[anydoor7/tslink](https://github.com/anydoor7/tslink/blob/main/skills/tslink-preview/SKILL.md).
Edit it there first; this repository carries a byte-identical copy checked
by CI on pushes, pull requests and a daily schedule.

Licensed under [Apache 2.0](LICENSE). TSLink is an independent project and
is not made or endorsed by Tailscale.
