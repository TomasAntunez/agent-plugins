---
name: commit
description: Plan and create git commits. Use when the user asks to commit changes. Splits the work into separate logical commits, shows the plan, and commits once the user confirms.
---

Turn the changes in the working tree into commits that each hold one logical change, with minimal Conventional Commits messages. Show the plan first and commit only after the user confirms.

## 1. Scope

If the user named what to commit, the scope is only that. Otherwise it is every change in the working tree: staged, unstaged, and untracked.

Ignore what is currently staged. You decide what goes into each commit.

## 2. Read the changes

Read the full diff against HEAD and the untracked files.

## 3. Group

One commit per logical change: something that could be reviewed or reverted on its own. Keep a change together with its tests and docs. Keep unrelated changes apart, such as a fix, a refactor, and a formatting pass, even when they touch the same file; in that case split the file's changes across commits. Order the commits so that each one works without the ones after it.

Leave out anything that should not be committed, like secrets, local configuration, build output, and debugging leftovers, and anything outside the scope.

## 4. Show the plan and wait

List each commit with its full message and files, marking files that are only partly included, then list what you left out and why:

```
1. fix(auth): reject expired tokens
   src/auth/token.ts (expiry check only)
   src/auth/token.test.ts
2. refactor(auth): extract header parsing
   src/auth/token.ts (parseHeader)
Left out: .env.local (secrets), scratch.py (not part of this work)
```

Do not change the index or the history before the user confirms. If they ask for changes, show the revised plan and wait again. Skip the wait only if the user already said to commit without asking.

## 5. Commit

For each commit, stage exactly its content, check that the staged diff matches the plan, and commit.

- Unstage everything first. Never modify or discard working tree files.
- To include part of a file, write a patch with only the hunks for that commit and apply it with `git apply --cached`. Interactive staging is not available.
- If a hook fails, stop and show its output. Do not bypass hooks, amend, or push.

Finish by listing the commits created and anything still uncommitted.

## Message format

Always Conventional Commits, in English. The diff explains the change; the message only names it.

```
type(scope): subject
```

- `type`: feat, fix, refactor, perf, docs, test, build, ci, or chore.
- `scope`: the area changed. Omit it when there is no obvious one.
- `subject`: imperative, lowercase, no period. Name what the change does in as few words as it takes: aim for 50 characters, never more than 72.
- Body: none by default. Add one only when the diff cannot show the reason or something should stay on record, such as a non-obvious why or a trade-off. Keep it to a line or two.
- Breaking change: `!` after the type and a `BREAKING CHANGE:` footer.
- No trailers: no `Co-Authored-By` and no generated-by lines.
