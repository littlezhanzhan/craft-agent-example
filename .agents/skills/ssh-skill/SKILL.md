---
name: "SSH Skill"
description: "Use curated Python wrappers for SSH execution, upload/download, tunneling, cluster runs, and server lookup from a portable project-level skill. Prefer existing ~/.ssh/config aliases and key-based auth."
alwaysAllow:
  - "Bash"
  - "Read"
---

# SSH Skill

Use this skill when the user wants to work with remote servers over SSH.

This project-level skill wraps SSH operations through curated Python scripts bundled with the skill instead of using raw `ssh` or `scp` commands directly.

## Installation location

This shared version is designed to live in a project-level skill directory:

`<project-root>/.agents/skills/ssh-skill/`

All commands below assume the scripts live at:

`$HOME/.agents/skills/ssh-skill/scripts`

or inside a checked-out project at:

`.agents/skills/ssh-skill/scripts`

When executing, prefer the path that matches the actual installation location.

## Primary goals

Use this skill for:
- remote command execution
- listing or finding configured host aliases
- upload/download over SSH
- server-to-server transfer
- SSH tunnel / local port forwarding
- batch execution against multiple hosts

## Security and safety rules

Follow these rules strictly:

1. **Prefer existing aliases from `~/.ssh/config`**
   - First use existing configured aliases.
   - Do not assume raw host/IP login should be done ad hoc when an alias likely exists.

2. **Default to key-based auth / ssh-agent**
   - Prefer OpenSSH key auth and ssh-agent.
   - Do not assume interactive password or passphrase entry will work.

3. **Do not modify SSH config unless the user explicitly asks**
   - By default, this skill is for using existing config, not editing it.
   - Do not create, update, or delete SSH config entries without explicit user approval.

4. **Do not store plaintext passwords**
   - Do not add plaintext passwords to `~/.ssh/config` comments.
   - If password-based auth is requested, explain the risk and prefer key migration instead.

5. **Confirm destructive or broad-impact operations**
   - Confirm before restart, stop, delete, overwrite, mass deploy, or cluster-wide mutations.
   - Confirm before recursive uploads/downloads that may overwrite data.

6. **Do not misstate status**
   - Only report success after checking the command result.
   - Clearly distinguish prepared / attempted / completed / blocked.

7. **Stop after repeated failure**
   - If the same operation class fails twice, stop and switch strategy.
   - Do not mechanically retry SSH, upload, or tunnel operations.

## Preferred command patterns

> Replace `SCRIPTS` with the actual scripts directory, for example:
> - `"$HOME/.agents/skills/ssh-skill/scripts"`
> - `".agents/skills/ssh-skill/scripts"`

### List configured servers

```bash
python "SCRIPTS/ssh_config_manager_v3.py" list-servers
```

### Find configured servers

```bash
python "SCRIPTS/ssh_config_manager_v3.py" find "<keyword>"
```

### Execute a remote command

```bash
python "SCRIPTS/ssh_execute.py" <alias> "<command>"
```

Optional flags:
- `--timeout <seconds>`
- `--no-daemon`

### Upload a file or directory

**macOS / Linux**

```bash
python "SCRIPTS/ssh_upload.py" <alias> "<local_path>" "<remote_path>"
```

**Windows Git Bash**

```bash
MSYS_NO_PATHCONV=1 python "SCRIPTS/ssh_upload.py" <alias> "<local_path>" "<remote_path>"
```

Optional flags:
- `--resume`
- `--recursive`
- `--no-progress`

### Download a file or directory

**macOS / Linux**

```bash
python "SCRIPTS/ssh_download.py" <alias> "<remote_path>" "<local_path>"
```

**Windows Git Bash**

```bash
MSYS_NO_PATHCONV=1 python "SCRIPTS/ssh_download.py" <alias> "<remote_path>" "<local_path>"
```

Optional flags:
- `--resume`
- `--recursive`
- `--no-progress`

### Server-to-server transfer

**macOS / Linux**

```bash
python "SCRIPTS/ssh_server_transfer.py" <source_alias> "<source_path>" <target_alias> "<target_path>"
```

**Windows Git Bash**

```bash
MSYS_NO_PATHCONV=1 python "SCRIPTS/ssh_server_transfer.py" <source_alias> "<source_path>" <target_alias> "<target_path>"
```

Optional flags:
- `--mode auto|direct|stream|hybrid`
- `--use-rsync`
- `--no-progress`
- `--size-threshold <MB>`
- `--timeout <seconds>`

### Cluster command execution

```bash
python "SCRIPTS/ssh_cluster.py" "<command>" --parallel
```

Use environment, tag, or host filters when possible.

### SSH tunnel

```bash
python "SCRIPTS/ssh_tunnel.py" start <alias> --remote-port <port>
```

## Execution guidance

### For simple server inspection

Prefer combining read-only checks into one remote command when they target the same host, for example:

```bash
python "SCRIPTS/ssh_execute.py" my-host "hostname && whoami && uptime && df -h"
```

### For high-risk actions

Before running any command that changes server state, confirm:
- target host(s)
- exact command
- expected blast radius

### For tunnels

After creating a tunnel, report:
- tunnel id if provided
- local port
- remote host/port
- how to use it locally

## Dependencies

- Python 3.8+
- `paramiko`
- system OpenSSH client (`ssh`)
- configured aliases in `~/.ssh/config`

## Default output expectations

When using this skill, summarize:
- target alias or aliases
- operation type
- whether it succeeded
- key stdout/stderr takeaways
- any next step the user should take

## Explicitly not exposed by default in this shared version

This shared adaptation does **not** expose these as normal default actions:
- storing plaintext passwords in SSH config comments
- deleting SSH host entries
- rewriting the user SSH config automatically
- migration helper scripts from old custom formats

If the user explicitly wants config editing, treat it as a separate, higher-risk task and confirm first.

## Attribution

This shared skill is adapted from:
- <https://github.com/badseal/ssh-skill>

It was adjusted for Craft Agent project-level skill usage, portability, and safer default guidance.
