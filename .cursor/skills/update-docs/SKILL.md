---
name: update-docs
description: Updates markdown documentation to reflect code changes on the current git branch. Use when the user wants to update docs, sync documentation with code changes, or keep a README, changelog, or guide current after modifications. The user provides the path to the documentation file to update.
---

# Update Documentation from Branch Changes

Update a markdown documentation file so it accurately reflects the code changes on the current branch.

## Inputs

The user must provide:
- **Documentation file path** — the `.md` file to update.

Optional (infer when not provided):
- **Base branch** — defaults to `main` or `master` (whichever exists). Use the upstream tracking branch if one is set.

## Workflow

Copy this checklist and track progress:

```
Task Progress:
- [ ] Step 1: Identify base branch and gather diff
- [ ] Step 2: Read the target documentation file
- [ ] Step 3: Analyze changes and map to doc sections
- [ ] Step 4: Update documentation
- [ ] Step 5: Show summary of changes
```

### Step 1: Identify base branch and gather diff

Determine the comparison base:

```bash
# Check for upstream tracking branch
git rev-parse --abbrev-ref @{upstream} 2>/dev/null

# If no upstream, detect default branch
git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null || echo "origin/main"
```

Then collect the full diff and list of changed files:

```bash
# List changed files (paths only)
git diff --name-only <base>...HEAD

# Full diff for context
git diff <base>...HEAD

# Commit messages on this branch
git log --oneline <base>...HEAD
```

### Step 2: Read the target documentation file

Read the documentation file the user specified. If the file does not exist, confirm with the user whether to create it.

### Step 3: Analyze changes and map to doc sections

For each changed file, determine:

1. **What changed** — new functions, modified signatures, removed features, config changes, dependency additions, API changes.
2. **Which doc section is affected** — match changes to existing headings/sections in the documentation.
3. **What is missing** — identify sections that need to be added for new functionality.

Categorize updates:

| Category | Action |
|----------|--------|
| New feature / endpoint / component | Add new section or subsection |
| Changed behavior / API signature | Update existing description |
| Removed feature | Remove or mark as deprecated |
| New dependency | Update installation / setup section |
| Config change | Update configuration section |
| Bug fix (user-facing) | Update known-issues or changelog section if present |

### Step 4: Update documentation

Apply changes to the documentation file following these rules:

- **Preserve the existing style and tone.** Match heading levels, list formatting, and prose style already in the file.
- **Minimize diff size.** Only change what is necessary — do not rewrite sections that are still accurate.
- **Keep technical accuracy.** Reference actual function names, parameters, types, and file paths from the diff.
- **Add, don't fabricate.** Only document behavior visible in the code changes. If uncertain about intent, add a `<!-- TODO: verify -->` comment.
- **Maintain heading hierarchy.** Do not skip heading levels or restructure unless the user asks.

### Step 5: Show summary of changes

After editing, present a concise summary:

```markdown
## Documentation Update Summary

**File:** `path/to/doc.md`
**Base:** `main` → `HEAD` (N commits)

### Changes made
- Updated section "Installation" — added new dependency `foo`
- Added section "Authentication" — documents new auth middleware
- Removed reference to deprecated `legacyHandler`

### Sections unchanged (still accurate)
- "Getting Started"
- "API Reference > /users"
```

## Edge Cases

- **No relevant changes**: If the diff does not affect anything described in the documentation, report that no updates are needed.
- **Doc file does not exist**: Ask the user whether to create it. If yes, scaffold headings based on the branch changes.
- **Ambiguous mapping**: When a code change could affect multiple doc sections, update all relevant sections and flag them in the summary.
