# External Agents

`@eduwork/dsh-external-agents` · v0.1.1 · MulanPSL-2.0

An optional EduWork plugin that lets a DSH desktop host discover and connect local external agents — the Codex runtime and the Cursor Agent CLI — plus any ACP agent you add by hand over stdio. Connections are checked, enabled and disabled from a settings form, and once enabled the agent is reachable from the main chat as a DSH subagent.

Delegation itself is not reimplemented here: the plugin only adds discovery, connection checks and a configuration surface, while the official Codex/ACP providers, subprocess handling, permissions, session isolation and result delivery come from DSH. The companion official providers must match the version of the host that runs them.

## Highlights

- Passively scans common install directories (`~/.local/bin`, `~/.cursor/bin`) and `PATH` for `codex` and `agent` binaries without executing them, and also treats an installed `/Applications/Cursor.app` as a Cursor candidate on macOS.
- Probes a connection only on explicit request: Codex runs the official CLI's `login status`, while an ACP agent receives a single `initialize` handshake with a 10 s timeout and a 64 KiB output cap.
- Requires a successful check before a connection can be enabled, and refuses to remove a manually added agent until it is disabled.
- Persists connection settings through a DSH storage domain (`eduwork_external_agents`, version 1, table `agents`) so enabled state survives a restart, while check results are deliberately not persisted.
- Registers each enabled connection with the official `@deepseek-ai/dsh-subagent-codex` or `@deepseek-ai/dsh-subagent-acp` provider and exposes it through `@deepseek-ai/dsh-tool-subagent` as `subagent_<provider>`.
- Serializes list/add/check/enable/remove operations through one remote `manage` call, so a stale probe cannot enable a command that was just replaced.
- Lets other distributions add their own stdio ACP agent by absolute executable path plus a JSON argument array, without launching it at save time.

## Install

Install this directory as a local DSH plugin bundle. From a shell:

```bash
dsh plugin --profile web add /absolute/path/to/dsh-external-agents
```

Or add the directory through the **Plugins** panel in DSH (local bundle). Restart the DSH app after installation; plugins are activated from the bundle patch (`cordis.patch.yml`).

## Configuration

No configuration keys: the bundle patch inserts the plugin with defaults. The patch is marked optional and does not change the edition's default composition; the settings form stores per-connection entries in the plugin's own storage domain rather than in the bundle patch.

## Requirements

- DeepSeek Harness >=0.2.1 <0.3.0
- Node >=24.22.1
- Peer packages: @deepseek-ai/dsh-subagent-codex, @deepseek-ai/dsh-subagent-acp, zod, react, @deepseek-ai/dsh-tool-subagent, @deepseek-ai/dsh-storage-domain, @deepseek-ai/dsh-typert-protocol

## Notes and limits

- Targets a local desktop Host only: there is no remote-host discovery, no automatic installation or sign-in, and no task resumption. macOS is the first acceptance platform; Windows and Linux still need native acceptance.
- A successful ACP handshake proves protocol compatibility, not authentication or that a real task will run, and Codex sign-in through the main model does not carry over to the agent — sign in through each agent's own entry point first. The plugin does not store or forward login credentials and does not put secrets in arguments.
- Codex uses `permissionMode: never` and ACP uses `permission: reject`; that rejects interactive authorization, it is not a forced read-only mode or a separate filesystem sandbox.
- This directory is a source snapshot: no `lib/` build output is included, so the `lib/index.js` entry declared in `package.json` does not exist yet. Build the package in the EduWork tree before installing, otherwise DSH reports a missing bundle entry.
- The Codex runtime is the one pinned by the official package, and a discovered CLI path is only a reference. If an official Codex bundle is already installed, disable it first to avoid registering the same provider twice; Cursor requires the Agent CLI, not just the editor.

## 中文文档

[README.zh-CN.md](./README.zh-CN.md)

## License

MulanPSL-2.0 — see [LICENSE](./LICENSE) and, where present, [LICENSE-MIT](./LICENSE-MIT). Extracted from the ECNU EduWork distribution (https://github.com/ECNU/EduWork); upstream copyright and license notices are preserved.
