# AGENTS.md — Codex / VS Code Agent Rules (Jekyll Site, WSL/Bash)

## 0) Prime Directive

Be safe, predictable, and reversible. Prefer small, reviewable diffs.
Agents must work in a way that supports auditability, reproducibility, and human review.

---

## 1) Execution Environment

* Default shell: **bash** (assume WSL).
* Prefer POSIX commands and paths.
* Do not require PowerShell.
* Assume Linux-like environment even if running on Windows.

---

## 2) Isolation-First Workflow (Required)

All agent work must happen in an **isolated environment** so the main working tree and protected branches remain stable.

Acceptable isolation includes:

* Dedicated Git branches
* Git worktrees
* Disposable local clones
* Cloud or ephemeral environments (Codespaces, CI agents, etc.)

Agents **must never directly modify protected branches**.

---

### 2.1 Determine current state (when working locally)

Before changes in the active working directory, report:

* current branch: `git branch --show-current`
* whether staging is non-empty: `git diff --cached --name-only`
* whether working tree is dirty: `git status --porcelain`

---

### 2.2 Local safety check

If working locally and staged changes exist:

* STOP and report exactly what is staged.
* Ask how to proceed.
* Do not modify staged content unless instructed.

If the working tree is dirty:

* List pre-existing files.
* Clearly mark **pre-existing vs agent-modified** files.

These checks are not required in isolated or disposable environments.

---

### 2.3 Branch / workspace strategy

Preferred branch naming:

```
agent/<short-task-slug>
```

Agents may:

* Create branches automatically.
* Create commits freely in their own branch or workspace.
* Use checkpoints to support reasoning, testing, and rollback.

Agents must not:

* Commit to main/master.
* Rewrite protected history.
* Force push to protected branches.

---

### 2.4 Merge and PR policy

Agents must not merge automatically unless explicitly authorized.

Default workflow:

1. Create isolated branch or workspace.
2. Commit progress.
3. Validate changes.
4. Produce a PR or patch for review.

If asked to merge:

* Provide commands but do not execute unless clearly authorized.

---

## 3) Autonomy Levels

Agents should adapt behavior based on requested autonomy:

**Level 0 — Advisory**

* Suggestions only.
* No file changes.

**Level 1 — Interactive editing**

* Edit files but ask before significant or risky changes.

**Level 2 — Autonomous branch + PR (default)**

* Work independently in isolation.
* Validate.
* Produce PR or patch.

**Level 3 — Policy-based auto-merge**

* Only when explicitly configured and tests pass.

Default autonomy: **Level 2**.

---

## 4) Operational Rules While Editing

* Keep changes minimal and task-scoped.
* Avoid drive-by refactors and broad formatting rewrites.
* Follow existing repo conventions.
* Warn before opinionated or structural changes.
* Prefer incremental commits.

---

## 5) Jekyll Best Practices + Nonstandard Warning

Assume standard Jekyll unless the repo indicates otherwise.

### 5.1 Structure

* `_layouts/`
* `_includes/`
* `_sass/`
* `assets/`
* `_posts/YYYY-MM-DD-title.md`

### 5.2 Collections

* Define in `_config.yml`.
* Use `_<collection>/`.

### 5.3 Nonstandard detection

If deviating from common Jekyll practice:

1. Explain why.
2. Offer a standard alternative.
3. Ask before proceeding.

---

## 6) Front Matter & Content Style

Prefer minimal, valid YAML.

Common keys:

* `title`
* `layout`
* `permalink` (only when needed)
* `description` or `excerpt`
* ordering keys only if already used.

Formatting:

* Logical heading hierarchy.
* No trailing whitespace.
* Do not reflow text unless asked.

---

## 7) Build and Validation (Required)

Agents must validate before declaring work complete.

### 7.1 Always

```
bundle exec jekyll build
```

### 7.2 Dependencies

* Run `bundle install` only if required.
* Do not upgrade gems casually.
* Avoid changing `Gemfile.lock` unless needed.

### 7.3 Optional preview

Ask before running:

```
bundle exec jekyll serve
```

---

## 8) Link Hygiene (Required)

Baseline:

* Check internal links on modified pages.
* Verify navigation and permalinks.

If link tooling exists, use it.

If not, propose:

* htmlproofer
* lychee

Do not introduce tooling without approval.

---

## 9) Reporting Format (Every Task)

Provide:

1. Start state
2. Isolation strategy used
3. What changed
4. Files touched
5. Validation results
6. Notes, warnings, assumptions

---

## 10) Cost and Model Awareness

Agents should:

* Prefer local or lower-cost models when appropriate.
* Escalate to stronger cloud models only when needed.
* Minimize unnecessary tool or model calls.

---

## 11) Guardrails / Do-Not-Do

* No changes to protected branches.
* No automatic merges unless authorized.
* No large-scale reformatting.
* No permalink or URL strategy changes without approval.
* No new CI or build tools without approval.
* Minimal PowerShell - use only if needed because cannot be done with bash - even then ask first


Those each emphasize slightly different tradeoffs.
