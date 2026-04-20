# Agent Team Workflow

Prescribed workflow for all agent team work in the Unifi.NET SDK repository. Every phase is
mandatory — no shortcuts.

---

## Phase 0: Discovery

Before any work, establish a **unique team identity** tied to a GitHub issue number.

### Step 1: Determine the issue number

| Scenario | Action |
|----------|--------|
| User provided `#NNN` | Use that number |
| User described work but no issue exists | Create one now |
| Work is exploratory / spike | Create a spike issue |

```bash
gh issue create \
  --title "Brief description" \
  --body "Tracking issue for team work." \
  --repo iamadamreed/Unifi.NET
```

### Step 2: Set the team namespace

All identifiers derive from the issue number:

| Identifier | Format | Example |
|-----------|--------|---------|
| Team name | `unifi-{issue-number}` | `unifi-42` |
| Worktree path | `worktrees/{issue-number}-{name}` | `worktrees/42-access` |
| Branch | `{type}/{issue-number}-{kebab-desc}` | `feat/42-visitor-endpoints` |

---

## Parallelism Model

| Agent Type | Parallelism | Isolation | Coordination |
|-----------|-------------|-----------|--------------|
| Researchers | Fully parallel | Read-only on main repo | Report to team-lead only |
| Workers | Fully parallel | Dedicated git worktree each | Message team-lead only |
| Reviewers | Parallel across PRs | Main repo (read-only) | Message worker + team-lead |
| Merges | Strictly sequential | N/A | Team-lead only |

**Key principles:**
- Every worker gets its own git worktree — no sharing
- Workers must NEVER touch files outside their domain paths
- Shared files (`Unifi.NET.sln`, `Directory.Packages.props`) will conflict between parallel
  workers — resolve at merge time, not during implementation
- Workers do NOT coordinate with each other directly

---

## Phase 1: Research

Spawn researcher agents for large or unfamiliar tasks. Researchers are read-only.

### Researcher output format

```
## Worker Task: {scope}

### Steps
1. **{Action verb} {what}**
   - Files: `path/to/file.cs`
   - Details: {instructions}

### Decisions Needed
- {Decision}: {options}

### Standards Compliance
- AOT warnings: {status}
- Missing registrations: {list}
```

---

## Phase 2: Planning

### Step 1: Create branches and worktrees

Branch naming: `{type}/{issue-number}-{kebab-description}`
Types: `feat`, `fix`, `chore`, `spike`, `refactor`

```bash
cd ~/dev/Unifi.NET && git checkout main && git pull
git worktree add worktrees/{issue-number}-{name} -b {branch-name} main
git worktree list
```

### Step 2: Spawn and engage agents

#### Worker prompt template

```
You are the @{name} domain expert on unifi-{issue-number}.

## Environment
- Worktree: {absolute-worktree-path}
- `cd {absolute-worktree-path}` as your FIRST command
- Prefix every bash command with `cd {absolute-worktree-path} &&`
- Use `~/.dotnet/dotnet` for all dotnet commands
- NEVER run `git checkout` — you are already on the correct branch
- Stage specific files only (never `git add -A`)

## Standards
- Commit format: `[#{issue-number}] Brief description`
- After committing: push and open PR targeting `main`

## Domain ownership
You own ONLY these paths: {domain-paths}
If you need changes outside your paths, message team-lead.

## Skills
Load these skills as needed: {skill-list}

## Task
{task-description}

## Completion
When done:
1. Mark your task(s) complete via TaskUpdate
2. SendMessage team-lead: PR number, files changed, any blockers
3. Wait for further instructions
```

#### Reviewer spawn (model: opus)

```
Review PR #{pr-number} for unifi-{issue-number}.

## PR
- URL: {url}
- Branch: {branch-name}
- Worker: @{worker-name}

## Instructions
1. Verify zero AOT warnings (`~/.dotnet/dotnet publish -c Release -r linux-x64`)
2. Cross-reference models against API documentation
3. Check JSON context registrations are complete
4. Verify test coverage
5. SendMessage violations to @{worker-name} with file:line
6. SendMessage summary to team-lead (approved/blocked)
```

---

## Phase 3: Implementation

Workers implement in isolated worktrees.

### Pre-flight checks

1. **Verify branch**: `git branch --show-current`
2. **Check NuGet versions**: verify packages in `Directory.Packages.props` exist
3. **Check source branch**: confirm not on main

### Build verification

```bash
# Build full solution
~/.dotnet/dotnet build Unifi.NET.sln

# Verify AOT compatibility (MANDATORY before PR)
~/.dotnet/dotnet publish -c Release -r linux-x64 --verbosity detailed 2>&1 | grep -i warning
# Must return nothing

# Run tests
~/.dotnet/dotnet test
```

### Commit and push

```bash
git commit -m "$(cat <<'EOF'
[#{issue}] Brief description
EOF
)"

git push -u origin {branch-name}
```

---

## Phase 4: PR Lifecycle

### Step 1: Worker opens PR

```bash
gh pr create --title "[#{issue}] Brief description" --body "$(cat <<'EOF'
## Summary
[1-3 bullet points]

## Changes
- [Change 1]
- [Change 2]

## Verification
- **Build:** Passes locally
- **AOT:** Zero warnings from `dotnet publish -r linux-x64`
- **Tests:** All pass

## Checklist
- [ ] AOT compatible (zero warnings)
- [ ] JSON contexts updated
- [ ] API_IMPLEMENTATION_STATUS.md updated
- [ ] Tests added
EOF
)" --base main
```

### Step 2: Team lead assigns reviewer

Spawn a fresh `code-reviewer` instance per PR.

### Step 3: Reviewer audits

The reviewer checks against the full review checklist (AOT, STJ, API fidelity, tests, design).

### Step 4: Worker fixes violations

Workers fix issues, verify AOT again, push.

### Step 5: Team lead merges

```bash
gh pr merge {number} --squash --delete-branch
```

---

## Phase 5: Merge & Cleanup

### Sequential merging

When multiple PRs exist, merge one at a time:
1. Merge first PR
2. Rebase subsequent PRs onto updated main
3. Worker re-verifies AOT + tests after rebase
4. Repeat

### Worktree cleanup

```bash
git worktree remove worktrees/{issue-number}-{name}
git worktree list
```

Then call `TeamDelete`.

---

## Rules & Gotchas

### AOT is non-negotiable

- Zero AOT warnings policy. If `dotnet publish -r linux-x64` produces warnings, the PR is blocked.
- Every model must be registered in a `JsonSerializerContext`
- No reflection, no dynamic, no `Assembly.GetTypes()`

### Build commands

- Use `~/.dotnet/dotnet` — not `dotnet` (JetBrains Rider manages .NET on ARM64 Mac)
- Build full solution: `~/.dotnet/dotnet build Unifi.NET.sln`
- Run tests: `~/.dotnet/dotnet test`
- AOT verify: `~/.dotnet/dotnet publish -c Release -r linux-x64`

### Branch flow

```
feature branch → PR → main
```

- All work starts on a feature branch off main
- All feature branches merge to main via PR (squash)
- Never push directly to main

### JSON Serialization

- System.Text.Json source generators ONLY
- Service-specific contexts: `UserJsonContext`, `DoorJsonContext`, etc.
- Combined via `JsonTypeInfoResolver.Combine()` in `UnifiAccessJsonContext`
- Runtime resolution: `_jsonOptions.GetTypeInfo(typeof(T))` → `JsonTypeInfo<T>`
- Every DTO needs `[JsonPropertyName("snake_case")]` attributes

### API Implementation Tracking

- `API_IMPLEMENTATION_STATUS.md` is the source of truth for endpoint coverage
- Update it when implementing or completing endpoints
- Track: endpoint URL, implementation status, test status, priority level
