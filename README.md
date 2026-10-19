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

Claude Code fetches this plugin from this GitHub repository, for example
through the `anydoor7/tslink` marketplace. That marketplace pins a reviewed
commit; pushing here alone does not update its users. The plugin launches the
locally installed
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
When explicitly requested, the MCP tools can:

- Publish apps privately through Tailscale or publicly through Funnel, configure
  services, remove apps and their owned Tailscale devices (`share`, `add`, `unshare`).
- Invite people to the tailnet or share service devices, list, resend and revoke
  invitations (`invite_user`, `invite_device`, `invite_list`, `invite_resend`, `invite_revoke`).
- Add, update and remove people, grant or revoke app access, set access deadlines,
  and approve or deny access requests (`people_*`, `extend`, `requests_*`).
- Set service tags, enable or disable the portal, edit gateway routes, and
  create or revoke browser guest links (`tags_set`, `portal_*`, `guest_*`).
- Apply service templates and app recipes, restart gateway nodes, configure
  health checks, and inspect app status (`template_*`, `recipe_*`, `app_restart`, `health*`).
- Read daemon logs, the mutation journal and access history (`logs`, `mcp_audit`,
  `access_log`, `access_summary`).

These capabilities follow the tool list in
[`cmd/mcp_titles.go`](https://github.com/anydoor7/tslink/blob/main/cmd/mcp_titles.go).
Some operations change local configuration; invitations and administrative
operations can send data to the Tailscale API. See the
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
by CI on pushes to main and pull requests, with a best-effort daily schedule
(GitHub can disable schedules on inactive repositories). The tslink repository's
CI also checks the skill and MCP command at its pinned plugin commit. To ship an
update through the `anydoor7/tslink` marketplace, push the plugin commit first,
then open a reviewed PR in `anydoor7/tslink` that bumps `source.sha` in
`.claude-plugin/marketplace.json` to that full commit SHA. Keep both skill copies
identical in that PR. After it merges, users update the marketplace and plugin.

Licensed under [Apache 2.0](LICENSE). TSLink is an independent project and
is not made or endorsed by Tailscale.
