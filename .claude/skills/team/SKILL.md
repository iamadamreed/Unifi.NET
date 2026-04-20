---
name: team
description: >
  Coordinates agent teams for Unifi.NET SDK development. Spawns domain-specific worker agents
  in isolated git worktrees with a code reviewer. Use when work spans multiple SDK projects
  or requires parallel development across UniFi product boundaries.
disable-model-invocation: true
argument-hint: "[task description or #issue-number]"
effort: max
allowed-tools:
  - TeamCreate
  - TeamDelete
  - Agent
  - SendMessage
  - TaskCreate
  - TaskGet
  - TaskList
  - TaskUpdate
  - TaskStop
  - Read
  - Glob
  - Grep
  - "Bash(git *)"
  - "Bash(gh *)"
  - "Bash(rm -rf *)"
  - "Bash(ls *)"
  - "Bash(cd *)"
---

## Current State (auto-detected)
- Worktrees: !`git worktree list`
- Branch: !`git branch --show-current`
- Active teams: !`ls -d ~/.claude/teams/unifi-* 2>/dev/null | xargs -I{} basename {} || echo "none"`
- Open PRs: !`gh pr list --state open --limit 5 --json number,title,headRefName --jq '.[] | "#\(.number) \(.title) [\(.headRefName)]"' 2>/dev/null || echo "none"`

You MUST use the TeamCreate tool and Agent tool with team_name to create a proper Agent Team.

**Read `${CLAUDE_SKILL_DIR}/workflow.md` for the complete prescribed workflow.** It defines all
phases (Discovery → Planning → Implementation → Review → Merge & Cleanup), the parallelism
model, model strategy, and rules. Follow it exactly.

## Phase 0: Discovery

Every team needs a **unique team name** derived from a GitHub issue number (`unifi-{issue-number}`).

| Identifier | Format | Example |
|-----------|--------|---------|
| Team name | `unifi-{issue-number}` | `unifi-42` |
| Worktree path | `worktrees/{issue-number}-{name}` | `worktrees/42-access` |
| Roster task | `unifi-{issue-number}-roster` | `unifi-42-roster` |
| Agent prompt | `"You are the @{name} domain expert on unifi-{issue-number}"` | `"You are the @access domain expert on unifi-42"` |

## Model strategy

| Role | Model | Context | Spawn pattern |
|------|-------|---------|---------------|
| Workers | `model: "sonnet"` | 200k | Spawn with full task — engage immediately |
| Researchers (broad) | `model: "opus"` | 1M | Cross-product, exploratory |
| Researchers (scoped) | `model: "sonnet"` | 200k | Single-product, known file patterns |
| Reviewer | `model: "opus"` | 1M | Spawn with full task — engage immediately |

**CRITICAL:** Always pass `model` explicitly. If omitted, agents inherit the parent's model.

## Domain agents

Read `${CLAUDE_SKILL_DIR}/domains.md` for the full registry of domain agents.

| Agent | Name | Domain paths |
|-------|------|-------------|
| @access | access | `Unifi.NET.Access/`, `Unifi.NET.Access.Tests/`, `Unifi.NET.AccessViewer/`, `Unifi.NET.Samples/` |
| @network | network | `Unifi.NET.Network/` |
| @protect | protect | `Unifi.NET.Protect/` |
| @site-manager | site-manager | `Unifi.NET.SiteManager/` |
| @common | common | `Unifi.NET.Common/`, `Unifi.NET.Common.Tests/` |

## Team state persistence

Create a `unifi-{issue-number}-roster` task immediately after team creation:

```
Team: unifi-{issue-number}
Issue: #{issue-number} — {description}

Agents:
  @{name} | {worktree-path} | {branch} | {status} | PR #{number}

Worktrees:
  worktrees/{issue-number}-{name} → {branch}

Current phase: {setup | implementing | reviewing | merging | cleanup}
```

## Step 1: Create the team

Call `TeamCreate` with:
- team_name: "unifi-{issue-number}"
- description: "Unifi.NET SDK development team"

## Step 2: Prepare worktrees

```bash
cd ~/dev/Unifi.NET && git checkout main && git pull
git worktree add worktrees/{issue-number}-{name} -b {branch-name} main
git worktree list
```

## Step 3: Spawn teammates

Use the worker prompt template from `workflow.md`. ALL agents run in background with
`team_name: "unifi-{issue-number}"`.

## Step 4: Act as team lead

See `workflow.md` for the full coordination protocol.

### Team Lead Rules

- **Never override the reviewer.** If AOT warnings exist, the PR is blocked. Period.
- **Never commit to main directly.** Everything goes through a PR.
- **Share the implementation plan with the reviewer.**
- **Never shut down the team without explicit user authorization.**
- **Never read PR comments yourself.** Delegate to workers.
- **Spawn new domain agents on the fly** when work crosses product boundaries.

### Cleanup

```bash
git worktree remove worktrees/{issue-number}-{name}
```

Then call `TeamDelete`.
