# Project-Specific Agent Instructions

Global engineering standards apply by default.
This file contains only project-specific rules.

---

## Site Framework

This repository is a Jekyll-based static site.

- Posts live in `_posts/`
- Pages use YAML front matter
- Respect existing front matter schema
- Preserve permalink structure
- Do not change `_config.yml` unless explicitly requested
- Do not introduce new plugins without approval

---

## Build & Validation

Before completion:

- Validate site builds cleanly
- Use the existing Jekyll build command (do not alter toolchain)
- Do not upgrade Ruby, Bundler, or Jekyll versions unless requested

---

## Content Discipline

- Preserve existing tone and structure of content
- Avoid reformatting unrelated pages
- Do not regenerate or bulk-rewrite posts

---

## Explicit Overrides

If this project needs to override a global rule, it must be declared here using:

`OVERRIDE: <description>`

If no override is declared, global rules remain authoritative.
