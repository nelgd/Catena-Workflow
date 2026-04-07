---
name: pr-description
description: Generates a PR description from the current branch's code changes using a standardized template.
model: Claude
---

## Workflow: Generate a PR Description

### Step 1: Identify the Base Branch

Run these commands to determine the branch context:

```
git branch --show-current
git log --oneline main..HEAD
```

- The current branch is the feature/fix branch.
- The base branch is `main` (or `master`). If neither exists, ask the user which branch to diff against.
- If there are no commits ahead of the base branch, inform the user and stop.

### Step 2: Gather the Changes

Run these commands to collect the full picture of what changed:

```
git diff main...HEAD --stat
git diff main...HEAD
git log main..HEAD --pretty=format:"%h %s"
```

Read the diff output carefully. Pay attention to:
- Which files were added, modified, or deleted.
- The nature of the changes (new feature, bug fix, refactor, config change, test addition, etc.).
- Any database/schema migrations included.
- Changes to public APIs, interfaces, or contracts.
- New dependencies added or removed.

### Step 3: Analyze and Categorize

From the gathered data, determine:
- **What** changed — a concise summary of the code changes.
- **Why** it changed — infer the goal from commit messages, file names, and the nature of the changes.
- **Definition of Done** — derive a checklist from what was actually done (features implemented, tests added, docs updated, etc.).
- **Implementation notes** — any notable technical decisions visible in the diff (new patterns, architectural choices, performance considerations).
- **Risks** — breaking changes, missing tests, large blast radius, or anything that warrants reviewer attention.
- **Related context** — references to issue numbers found in commit messages or branch names (e.g., `feat/JIRA-123-add-auth` → JIRA-123).

### Step 4: Fill In the Template

Using the analysis, populate this template. Every section must be filled in with meaningful content derived from the actual changes. Do not leave placeholder text. If a section truly does not apply, write "N/A" with a brief reason.

````markdown
## 📖 Description
<!-- A clear, concise summary of what this PR does. -->

---

## 🎯 Goal / Outcome
<!-- What should be achieved once this PR is merged? -->

---

## ✅ Definition of Done (DoD)
<!-- Checklist derived from the actual changes: -->
- [ ] Feature is implemented and works as expected
- [ ] Edge cases are handled
- [ ] Tests are added or updated (if applicable)
- [ ] Documentation is updated (if needed)
- [ ] Code is reviewed and approved

---

## 🛠️ Implementation Notes (Optional)
<!-- Technical details, architectural decisions, or constraints visible in the diff. -->

---

## 🔗 Related Issues / Links
<!-- Issue numbers, Jira tickets, or related PRs extracted from commits/branch name. -->

---

## 🚧 Risks / Open Questions (Optional)
<!-- Breaking changes, missing coverage, large blast radius, or open questions. -->

---

## 📝 Additional Context (Optional)
<!-- File-level change summary, screenshots, or background. -->
````

### Step 5: Return the Result

- Present the filled-in template to the user as a markdown code block so they can copy it directly.
- Do **not** create any files — only output the markdown in the chat.
- If the diff is very large (50+ files), summarize at a higher level and group changes by area/module rather than listing every file.
