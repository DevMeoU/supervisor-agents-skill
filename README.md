# Supervisor Agents Skill

An Anthropic-style Agent Skill for objective multi-agent supervision of code and architecture changes.

Use this skill when you want an AI agent to review git diffs against:

- system context and requirements
- implementation plans
- ADRs / architecture decision records
- repository conventions and constraints

The skill is review-only: it produces a structured compliance report and does not modify code.

## What it does

`supervisor-agents` guides an agent through five independent reviewer roles:

| Role | Purpose |
|------|---------|
| Diff Analyzer | Normalize changed files, diff themes, risk areas, missing tests/docs |
| Context Checker | Compare changes against system context, requirements, constraints, and non-goals |
| Plan Validator | Check whether implementation matches the intended plan or PR/task scope |
| ADR Auditor | Verify changes against active architectural decisions and detect ADR drift |
| Aggregator | Deduplicate findings and produce the final report |

## Output format

The skill instructs agents to report in this structure:

```markdown
## Summary

## Details

### Diff overview
### Compliance
### Violations
### Unknowns / gaps

## Recommendations
```

Findings are classified by severity:

- Blocker
- High
- Medium
- Low
- Informational

## Repository contents

```text
supervisor-agents-skill/
  SKILL.md      # Agent Skill definition
  LICENSE.txt   # License terms referenced by SKILL.md frontmatter
  README.md     # Human-facing repository documentation
```

## Skill frontmatter

The skill uses the common Anthropic-style format:

```yaml
---
name: supervisor-agents
description: "Use this skill whenever the user wants to supervise code or architecture changes by reviewing git diffs against system context, implementation plans, and ADRs..."
license: Proprietary. LICENSE.txt has complete terms
---
```

## Install manually

Copy the skill folder into your agent skills directory.

### Claude / Claude Code

```powershell
New-Item -ItemType Directory -Force ~/.claude/skills/supervisor-agents | Out-Null
Copy-Item ./SKILL.md ~/.claude/skills/supervisor-agents/SKILL.md -Force
Copy-Item ./LICENSE.txt ~/.claude/skills/supervisor-agents/LICENSE.txt -Force
```

### OpenClaw

```powershell
New-Item -ItemType Directory -Force ~/.openclaw/skills/supervisor-agents | Out-Null
Copy-Item ./SKILL.md ~/.openclaw/skills/supervisor-agents/SKILL.md -Force
Copy-Item ./LICENSE.txt ~/.openclaw/skills/supervisor-agents/LICENSE.txt -Force
```

OpenClaw config is schema-sensitive. Prefer using OpenClaw's supported skill configuration flow rather than editing `openclaw.json` by hand.

## Install with Agents Common Installer

This skill is also included as a bundled skill and catalog source in:

<https://github.com/DevMeoU/Agents-common-installer>

Example flow:

```powershell
# Clone/update catalogs
powershell -ExecutionPolicy Bypass -File ./agent-common-sync.ps1 -UpdateCatalog

# Copy this skill into common source
New-Item -ItemType Directory -Force ~/.agents/skills/supervisor-agents | Out-Null
Copy-Item ./skills/supervisor-agents/* ~/.agents/skills/supervisor-agents/ -Recurse -Force

# Sync to selected agents
powershell -ExecutionPolicy Bypass -File ./agent-common-sync.ps1 -Targets claude,openclaw,codex -Force
```

## Example prompts

Use this skill with prompts like:

```text
Review this PR against the plan and ADRs.
```

```text
Analyze the git diff and report architecture violations.
```

```text
Check whether these changes comply with the system context.
```

```text
Run a multi-agent supervision review for context, plan, ADR, and diff.
```

## Design principles

- Objective review, not implementation.
- Evidence-based findings only.
- Clear separation between compliance, violation, risk, and unknowns.
- Works with parallel subagents when available, but can also run as a single sequential review.
- Reports actionable recommendations without patching files.

## Validation

If you have the skill creator validator available, run:

```powershell
python <path-to-skill-creator>/scripts/quick_validate.py .
```

Expected result:

```text
Skill is valid!
```

## License

Proprietary. See [LICENSE.txt](LICENSE.txt).
