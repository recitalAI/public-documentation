# Documentation contribution guidelines

This repository contains the public reciTAL documentation published with GitBook. Apply these rules to every documentation change:

- Use the GitHub issue to define the requested scope and acceptance criteria, not as factual authority. Verify its product assumptions and flag conflicts with confirmed behavior. Preserve explicit requirements and keep the change scoped to the issue; make broader edits only when required for correctness or consistency.
- Write for both first-time users and AI/agent retrieval. Use descriptive headings, stable terminology, meaningful link text, and self-contained sections that remain understandable when retrieved independently.
- Follow the existing page language—currently French for customer documentation—unless the issue explicitly requests otherwise. Use clear, customer-facing language, preserve exact UI labels and API identifiers, and define unavoidable product-specific or internal terms on first use.
- Preserve GitBook-compatible Markdown, frontmatter, directives, and asset references. When changing headings or page paths, update `SUMMARY.md`, incoming links, and fragment identifiers; preserve existing public URLs where practical.
- Check surrounding and linked pages when changing terminology, concepts, links, or navigation so the documentation remains consistent.
- Verify links, commands, examples, API names, prerequisites, expected outcomes, and current product behavior against authoritative sources. Do not guess.
- Avoid unnecessary duplication. Maintain one canonical explanation and link to it from other pages when that gives readers enough context.
- Before finishing, review the rendered structure where practical and confirm that the change is concise, accurate, discoverable, and complete for the issue.
