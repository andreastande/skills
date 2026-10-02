---
name: commit
description: Commit and push with Conventional Commits. Suggests three subject lines matching your past commits, drafts the body, then stages, commits and pushes. Use when the user asks to commit, ship, or push their changes.
---

1. Run `git status` (never use `-uall`), `git diff --staged`, `git diff`, and `git log --oneline -25 --author="$(git config user.email)"` in parallel.
2. Analyze all changes (staged + unstaged), the recent commit history, and the relevant parts of our conversation to understand both the project's commit message style and the intent behind the changes.
3. Suggest exactly 3 subject lines — short, imperative, following Conventional Commits (`type: subject`, no scope) and matching the style of recent commits. Present them as a numbered list and ask me to pick one (or provide my own). Below the list, at the very bottom of the output, show the body you'll use in a code block: 2–3 lines wrapped at ~72 chars, explaining what changed and why. Show one body, not options; I may give feedback on it.
4. Once I pick a subject (and apply any body feedback), stage all relevant files (prefer naming specific files over `git add -A`), commit with the subject and body, then push to the current remote branch. Do NOT add a co-author trailer.
