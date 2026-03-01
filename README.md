# sec-engineers.github.io

This repository hosts a Jekyll-based static site. Additional tooling has been added to help
maintain spelling and link integrity during local development and in CI.

---

## Local developer workflow 🛠️

### Setup

1. **Install Node dependencies** (only required once per machine):
   ```sh
   npm install
   ```
   The `prepare` script will run `husky install` automatically and ensure Git hooks are active.

2. You can bypass Git hooks on rare occasions by using:
   ```sh
   git commit --no-verify
   ```

### Running checks manually

- `npm run spell` – spellchecks all Markdown files (`**/*.md`, including `_posts/**`). This is fast, works offline, and will exit non‑zero on any unknown words not listed in the custom dictionary.
- `npm run links` – runs a linkcheck using [linkinator](https://github.com/JustinBeckwith/linkinator) (chosen for ease of installation). It may perform network requests and therefore is *optional* locally. It honors the exclusions listed in `lychee.yml`.

  If you prefer to use **lychee** for consistency with CI you can install it separately (e.g. `cargo install lychee` or download a prebuilt binary) and then run it manually; the CI workflow makes use of the official `lycheeverse/lychee-action`.
- `npm run all` – convenience script to run both checks sequentially.

### Dictionary & exclusions

- Custom words for the spellchecker are stored in `.cspell.json` under the `words` array. Add domain-specific acronyms, product names, or other project-specific terms there.
- External URLs that are known to block bots or are intentionally flaky may be added to the `exclude` list in `lychee.yml`. Provide a brief comment when you add an entry explaining why it’s excluded.

### Git hooks

- A `pre-commit` hook is installed via [husky](https://typicode.github.io/husky/#/) and runs `npm run spell`. Any spelling errors will block the commit, preventing typos from entering the history. Hooks are automatically configured after running `npm install`.
- To bypass the hook (e.g. for an intentionally misspelled test case), use `git commit --no-verify`.

---

## Continuous Integration ☁️

Two GitHub Actions workflows are provided under `.github/workflows/`:

1. **spell.yml** – runs on `push` and `pull_request`. It executes `npm ci` and then `npm run spell`. Failures are reported but are _not_ intended to be required checks; spelling mistakes will not block merges.
2. **links.yml** – also runs on `push` and `pull_request`. In CI it invokes the `lycheeverse/lychee-action` directly (no Node dependency) but the job still calls `npm run links` as a convenience; a failing linkcheck will block merges to the protected branch.

Both workflows cache `node_modules` to speed up subsequent runs.

### Branch protection

To enforce link integrity:

1. Go to the repository **Settings → Branches → Branch protection rules**.
2. Add or edit a rule for the `main` (or primary) branch.
3. Enable **Require status checks to pass before merging**.
4. Select only the `links` job from the workflow list (it may appear as `links` or `linkcheck` depending on naming). Do **not** mark the spelling job as required.
5. Save the rule.

This ensures that pull requests cannot be merged unless the linkcheck workflow succeeds.

> ⚠️ The GitHub UI step must be performed manually; the workflows in this repo only provide the job definitions.

---

## Notes

- No Jekyll configuration, plugins, or Ruby gems were modified for these changes.
- Node tooling is strictly for the checks described above; there are no dependencies on the site build itself.
- The workflows use minimal permissions and only install development dependencies.

Feel free to extend the dictionary and exclusion lists as the site evolves. Happy writing! ✍️