# Documentation contribution guidelines

This repository contains the public reciTAL documentation published with GitBook. Apply these rules to every documentation change:

- Use the GitHub issue to define the requested scope and acceptance criteria, not as factual authority. Verify its product assumptions and flag conflicts with confirmed behavior. Preserve explicit requirements and keep the change scoped to the issue; make broader edits only when required for correctness or consistency.
- Write for both first-time users and AI/agent retrieval. Use descriptive headings, stable terminology, meaningful link text, and self-contained sections that remain understandable when retrieved independently.
- Follow the existing page language—currently French for customer documentation—unless the issue explicitly requests otherwise. Use clear, customer-facing language, preserve exact UI labels and API identifiers, and define unavoidable product-specific or internal terms on first use.
- Describe customer-observable capabilities and tasks, not internal activation, entitlement, packaging, or role mechanics, unless users need those mechanics to complete a task or understand why a capability is unavailable.
- Preserve GitBook-compatible Markdown, frontmatter, directives, and asset references. When changing headings or page paths, update `SUMMARY.md`, incoming links, and fragment identifiers; preserve existing public URLs where practical.
- Check surrounding and linked pages when changing terminology, concepts, links, or navigation so the documentation remains consistent.
- Verify links, commands, examples, API names, prerequisites, expected outcomes, and current product behavior against authoritative sources. Do not guess.
- Avoid unnecessary duplication. Maintain one canonical explanation and link to it from other pages when that gives readers enough context.
- Before finishing, review the rendered structure where practical and confirm that the change is concise, accurate, discoverable, and complete for the issue.

## Git workflow for agents

### Synchronize feature and PR branches by rebasing

- Before creating or synchronizing a branch, verify the remote head and the PR's actual base branch. For stacked PRs, use the immediate parent branch, not automatically `main`; preserve the parent-relative diff.
- Work on a feature branch, never directly on `main`. Fetch with `git fetch origin`, then synchronize with `git rebase origin/<actual-base>`. When a stacked PR needs an explicit `git rebase --onto` to preserve its focused diff, identify the old parent boundary and new base before proceeding.
- Never refresh a feature branch with `git merge main`, `git merge origin/main`, `git merge --no-commit origin/main`, a merge-producing `git pull`, or an automatic update-branch operation that creates a merge commit. If rebase conflicts occur, resolve them or stop and report the blocker; do not substitute a merge.
- Rebasing a published branch rewrites history. Before pushing rewritten commits, verify the remote repository, branch, and expected remote head, and obtain explicit authorization for that history rewrite. Use `--force-with-lease`, never plain `--force`; if the lease fails, stop and inspect the remote changes rather than overriding them. Never rewrite `main`.

### Integrate approved PRs into main with merge commits

Merge commits (`--no-ff`) are reserved for final integration of an approved PR into `main`, with explicit merge authorization. They are not a way to synchronize a working branch. Opening or updating a PR does not authorize merging it.

### Verify branch history and scope before delivery

After fetching, substitute the verified actual base branch for `<actual-base>`:

```bash
git merge-base --is-ancestor origin/<actual-base> HEAD
git rev-list --merges origin/<actual-base>..HEAD
git diff --check origin/<actual-base>...HEAD
git diff --name-status origin/<actual-base>...HEAD
git diff origin/<actual-base>...HEAD
git rev-parse HEAD origin/<actual-base>
```

The ancestry check must succeed, the merge list must be empty, and the whitespace check must pass. Review the changed files and full diff for issue scope; for stacked PRs, review only the immediate-parent-relative changes. Report validation results and the exact head and base commit IDs used.
