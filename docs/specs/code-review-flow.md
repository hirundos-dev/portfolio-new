# Code Review Flow

## Purpose

Provide a repeatable, layer-aware review process that checks correctness, architecture compliance, and quality for all platform layers before changes are merged.

## Recommended Review Sequence

### Step 1 — Understand context

- Read `AGENTS.md` to recall architecture rules, layer responsibilities, and pipeline standards.
- Identify which specs in `docs/specs/` are relevant to the change.
- Confirm what the change is supposed to accomplish before reading any diff.

### Step 2 — Inspect changes

```bash
git status                          # working-tree state
git diff --name-only HEAD           # files changed vs last commit
git diff HEAD                       # full unified diff
git log --oneline -10               # recent history for context
git diff main..HEAD --name-only     # files changed vs main branch
```

Group changed files by platform layer:

| Layer | Paths |
|-------|-------|
| Raw SQL / DDL | `sql/` |
| dbt models | `dbt/` |
| Airflow DAGs | `dags/` |
| Python ETL / shared | `src/` |
| Backend API | `apps/api/` |
| Frontend | `apps/web/` |
| Infrastructure | `docker-compose.yml`, `.env.example` |
| Documentation | `docs/specs/`, `AGENTS.md`, `README.md` |

### Step 3 — Layer-by-layer review

Review each affected layer in order of risk (highest first):

1. `sql/` — schema changes break existing data or require migrations
2. `dbt/` — model renames or removed columns break downstream dependencies
3. `dags/` — schedule or connection changes affect running pipelines
4. `src/` — shared code affects every consumer
5. `apps/api/` — API contract changes affect frontend and consumers
6. `apps/web/` — UI changes affect end users
7. `docker-compose.yml` — runtime changes affect the whole local environment
8. `docs/` — documentation drift signals missing alignment

Apply the `code-review-checklist` skill for each affected layer.

### Step 4 — Cross-cutting concerns

- **Security**: no hardcoded credentials, tokens, or API keys in any file.
- **Idempotency**: pipelines and DDL scripts safe to re-run without side-effects.
- **Documentation alignment**: if data contracts or interfaces changed, specs must also be updated.
- **Local runtime**: `docker-compose.yml` and `.env.example` stay aligned with code changes.
- **Observability**: logging and error handling present for external calls and long-running tasks.

### Step 5 — Produce recommendation plan

Classify every finding by severity and output the plan using the format below.

---

## Severity Levels

| Level | Meaning | Required action |
|-------|---------|-----------------|
| **Critical** | Data loss, broken correctness, exposed secret, broken pipeline | Must fix before merge |
| **Major** | Violates architecture rules, missing idempotency, contract drift | Should fix before merge |
| **Minor** | Naming, missing comments, small quality gaps | Fix or document trade-off |
| **Suggestion** | Refactor, improvement, future-work idea | Optional |

---

## Recommendation Plan Format

Use this exact structure when producing a review output:

```markdown
## Code Review: <scope, branch, or file set>

### Summary
One paragraph: what changed, which layers are affected, overall quality signal.

### Findings

#### 🔴 Critical
- [ ] **`<file>`:`<line>`** — <concise description of the problem and its impact>

#### 🟠 Major
- [ ] **`<file>`:`<line>`** — <description>

#### 🟡 Minor
- [ ] **`<file>`:`<line>`** — <description>

#### 💡 Suggestions
- [ ] **`<file>`** — <description>

### What looks good
- Brief bullet list of things done correctly or notably well.

### Next steps
Ordered action list. First item is the highest-severity finding.
1. …
2. …
```

---

## Scope Notes

- Default diff target is `HEAD` unless the user specifies a branch or commit.
- For PR reviews, compare against `main` unless another base is specified.
- If no git context is available, ask the user to paste the diff or specify file paths.
