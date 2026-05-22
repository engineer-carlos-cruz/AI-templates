---
description: Analyze repository changes and create atomic Conventional Commits in English.
---

You are creating commits from the current Git working tree.

Goal:
- Inspect all staged and unstaged changes.
- Group related modifications by logical intent or concern.
- Create one or more atomic commits using Conventional Commits.

Required workflow:
1. Review repository state with:
   - `git status --short`
   - `git diff`
   - `git diff --cached`
2. Derive change groups by purpose (feature, bug fix, docs, refactor, etc.).
3. For each group:
   - Stage only the files/hunks that belong to that group.
   - Write a high-quality commit message in English only.
   - Commit before moving to the next group.
4. Keep commits logically isolated and independent whenever possible.

Allowed Conventional Commit types:
- `feat`
- `fix`
- `refactor`
- `docs`
- `chore`
- `test`
- `style`
- `perf`
- `build`
- `ci`

Commit message quality rules:
- Message MUST be in English.
- Message MUST explain why the change exists, not only what changed.
- Use clear, professional, descriptive wording.
- Never use vague messages such as:
  - `update files`
  - `fixes`
  - `changes`
  - `misc updates`

Safety and execution restrictions:
- Never run tests.
- Never run build commands.
- Never start or execute the project.
- Never push changes to any remote repository.
- Only analyze changes, stage changes, and create commits.
- Do not modify files except what is strictly necessary for commit creation workflow.

Large repository behavior:
- Be deterministic and conservative when grouping changes.
- Prefer smaller atomic commits if grouping is ambiguous.
- Keep unrelated modifications in separate commits.
- Exclude generated/binary/vendor noise from commits unless it is clearly intentional.

Before each commit, briefly validate that the staged diff matches exactly one logical concern.
After all commits, show final `git status --short`.
