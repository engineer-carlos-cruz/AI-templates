---
description: Analyze changes and create atomic Conventional Commits in English.
agent: build
subtask: false
---

# Goal

Inspect the current Git working tree and produce one or more
atomic Conventional Commits that capture every intentional
change — no more, no less.

# Step 1 — Understand the current state

Run all three commands before touching the index:

`git status --short`
`git diff`
`git diff --cached`

If the working tree is completely clean (no staged or unstaged
changes), stop here and report: "Nothing to commit."

# Step 2 — Plan commit groups

Mentally partition every changed file/hunk into groups by
logical intent. Each group must satisfy:

• A single Conventional Commit type covers it entirely.
• No unrelated files share the group.
• Generated, binary, or vendor files are excluded unless
  the change is clearly intentional (e.g., lockfile update
  triggered by a dependency bump in the same commit).

When grouping is ambiguous, prefer more, smaller commits.

# Step 3 — For each group (in order)

3a. Stage only the relevant files/hunks

Use `git add <files>` or `git add -p` for partial staging.
Never stage unrelated files.

3b. Validate before committing

Run `git diff --cached --stat` and confirm:
• Every staged file belongs to this group's intent.
• No unintended file is included.

If validation fails, unstage the mismatched files and regroup.

3c. Write the commit message

Format:

  <type>[optional scope]: <short imperative summary>

  [optional body — explain WHY, not what]

  [optional footer — e.g. BREAKING CHANGE, Closes #n]

Rules:
• Language: English only.
• Subject line: ≤72 chars, imperative mood, no trailing period.
• Body: explain the motivation or context when it adds value.
• Forbidden vague subjects: "update files", "fix", "changes",
  "misc", "wip", "temp", "various updates".

3d. Commit

`git commit -m "<subject>" [-m "<body>"]`

Then move to the next group.

# Allowed Conventional Commit types

feat      new feature visible to users
fix       bug fix
refactor  internal restructuring, no behavior change
docs      documentation only
test      adding or updating tests
chore     maintenance, tooling, config
style     formatting, whitespace (no logic change)
perf      performance improvement
build     build system or external dependency changes
ci        CI/CD pipeline changes

Use feat!, fix!, etc. (with BREAKING CHANGE: footer) for
breaking changes.

# Hard restrictions — never do any of the following

• Run tests, build commands, or start the project.
• Push to any remote (git push is forbidden).
• Modify source files (only stage/commit operations allowed).
• Commit unrelated changes in the same commit.
• Use `git add .` or `git add -A` without reviewing what is staged.

# Finish

After all commits are done, run `git status --short` and show
the output. If any files remain uncommitted, explain briefly
why they were intentionally excluded.
