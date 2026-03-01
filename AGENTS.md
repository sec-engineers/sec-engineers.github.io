# AGENTS.md — Codex Agent Rules (Jekyll Site, WSL/Bash)

## 0) Prime Directive
Be safe, predictable, and reversible. Prefer small, reviewable diffs.

## 1) Execution Environment
- Default shell: **bash** (assume WSL).
- Prefer POSIX commands and paths.
- Do not require PowerShell.

## 2) Branch-First Workflow (Required)
All agent work must happen on a dedicated branch so main stays stable.

### 2.1 Determine current state
Before changes:
- Report:
  - current branch: `git branch --show-current`
  - whether staging is non-empty: `git diff --cached --name-only`
  - whether working tree is dirty: `git status --porcelain`

### 2.2 If staged changes exist, STOP
- If `git diff --cached` is non-empty, STOP and tell me exactly what is staged.
- Ask me what to do (unstage, commit myself, or abort).  
  **Do not proceed** until staging is empty.

### 2.3 Create/use an agent branch
If we are not already on an agent branch, create one:
- Branch naming (default): `codex/<short-task-slug>`
- Commands (typical):
  - `git switch -c codex/<task-slug>` (or `git checkout -b ...` if needed)

If the working tree is dirty when creating the branch:
- You may still create the branch, but you MUST:
  - list the pre-existing dirty files
  - clearly mark them as **pre-existing** vs **touched by the agent**
- Prefer to avoid mixing: if it looks risky, ask me to stash/commit first.

### 2.4 Never commit automatically
- Under **no circumstances** run `git commit`.
- You may stage files (`git add`) only if I explicitly ask.
- Committing is only done when I explicitly instruct: “commit now with message …”.

### 2.5 Never merge automatically
- Do not merge the agent branch into main/master.
- If asked, provide the exact commands I should run to merge/rebase, but do not execute.

## 3) Operational Rules While Editing
- Keep changes minimal and task-scoped.
- Avoid drive-by refactors and broad formatting rewrites.
- When in doubt, follow *this repo’s existing conventions* over generic advice.
- If a change is nonstandard or opinionated, warn first and offer options.

## 4) Jekyll Best Practices (Default) + “Nonstandard” Warning
Assume this is a standard Jekyll site unless the repo indicates otherwise.

### 4.1 Structure
- `_layouts/` for layouts
- `_includes/` for partials
- `_sass/` for Sass partials (if used)
- `assets/` for images/js/css (or the repo’s established pattern)
- Posts in `_posts/YYYY-MM-DD-title.md` (if blog-style posts are used)

### 4.2 Collections
- Prefer defining collections in `_config.yml`.
- Use `_<collection>/` directories for items.
- Provide sensible front matter and permalinks when needed.

### 4.3 Nonstandard detection
If I ask for a new subsection/content type and your approach deviates from:
- common Jekyll practice, OR
- this repo’s established conventions,
you must:
1) explain why it’s nonstandard,
2) propose the standard alternative,
3) ask which approach I want.

## 5) Front Matter & Content Style (Default Expectations)
Unless the repo shows a different style, prefer:

### 5.1 Front matter keys
- `title`: human readable
- `layout`: existing layout name (or propose one)
- `permalink`: only when needed; keep consistent style site-wide
- `description` or `excerpt`: if the site uses it
- `nav_order` / `order` / `weight`: only if the repo already uses ordering

### 5.2 Formatting
- Keep YAML front matter valid and minimal.
- Prefer Markdown headings in a logical hierarchy (`#`, `##`, `###`).
- Avoid trailing whitespace.
- Don’t reflow/re-wrap long paragraphs unless asked.

## 6) Build / Validate Before Declaring Done
Before saying “done”, run sanity checks in WSL.

### 6.1 Minimal checks (always)
- `bundle exec jekyll build`

### 6.2 Dependency handling
- If `bundle exec` fails due to missing gems:
  - run `bundle install` only if Gemfile/Gemfile.lock exist.
- Do not upgrade gems casually.
- Avoid changing `Gemfile.lock` unless required to get a working build.

### 6.3 Optional: Serve (ask first)
If changes are visual/content-related, ask:
- “Do you want me to run `bundle exec jekyll serve` so you can preview?”

## 7) Link Hygiene (Required)
Goal: catch broken internal links and obvious external link issues.

### 7.1 Baseline (always)
- Spot-check:
  - new/edited pages’ internal links
  - nav links affected by the change
  - any updated permalinks/paths

### 7.2 Stronger checks (when feasible)
If the repo already includes a link checker or CI job, use it.

If not, propose one of these “easy” options and ask before adding tools:
- **htmlproofer** (Ruby) to validate generated site output
- **lychee** (binary) for Markdown/HTML link checking

Rules:
- Do not introduce new tooling or CI steps without approval.
- If I approve, implement minimally and document how to run it in WSL.

## 8) Reporting Format (Every Task)
When you finish a step or task, report:

1) **Start state**
   - branch, staged? dirty? (list pre-existing dirty files)
2) **What changed**
   - bullet list
3) **Files touched**
   - explicit list
4) **Commands run + results**
   - build/link checks
5) **Notes / warnings**
   - nonstandard choices, assumptions, follow-ups

## 9) Guardrails / Do-Not-Do List
- No `git commit` unless explicitly instructed.
- No merges into main/master.
- No large-scale reformatting.
- No permalink/baseurl/site-url strategy changes without warning + approval.
- No adding build tools/CI steps without approval.
- No PowerShell.

  
