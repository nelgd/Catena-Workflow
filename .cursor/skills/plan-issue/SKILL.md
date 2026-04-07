---
name: plan-issue
description: Plans the implementation for a GitHub issue or bug by fetching issue details, exploring the codebase, and producing a grounded step-by-step implementation plan. Use when the user provides a GitHub issue URL and wants to plan how to implement it.
---

## Workflow: Plan an Issue Implementation

### Step 1: Fetch the Issue

Extract the owner, repo, and issue number from the provided GitHub URL, then fetch the issue details:

```
gh issue view <number> --repo <owner>/<repo> --json title,body,labels,comments,assignees
```

From the response, extract:
- **Title** and **body** — the core problem statement.
- **Labels** — bug, feature, enhancement, etc. This shapes the plan's structure.
- **Comments** — additional context, clarifications, or prior discussion that constrains the solution.

If the `gh` command fails (auth, permissions), fall back to `WebFetch` on the issue URL and parse what is available.

### Step 2: Identify Affected Areas

Based on the issue description, search the codebase to locate relevant code:

1. **Keyword search** — grep for domain terms, error messages, or identifiers mentioned in the issue.
2. **File discovery** — glob for files related to the feature area (e.g., `**/auth/**`, `**/*payment*`).
3. **Dependency mapping** — for each relevant file, identify its imports and callers to understand blast radius.

Read the most relevant files (limit to ~5–8 files to stay focused). Note:
- Current behavior and where it diverges from the desired behavior.
- Existing patterns, abstractions, and conventions the implementation should follow.
- Test files covering the affected code.

### Step 3: Analyze Constraints

Determine what constrains the implementation:
- **Existing tests** — what must keep passing?
- **Public APIs / interfaces** — what contracts exist with callers or consumers?
- **Database schema** — are migrations needed?
- **Dependencies** — are new packages required, or can existing ones cover it?
- **Related code** — are there similar patterns elsewhere that should be followed or unified?

### Step 4: Draft the Plan

Produce a plan using this template. Every section must be grounded in the actual codebase exploration from Steps 2–3. Do not speculate about file paths or structures — only reference what was found.

````markdown
# Implementation Plan: <issue title>

**Issue:** <link to GitHub issue>
**Type:** Bug fix | Feature | Enhancement | Refactor

## Problem Statement
<1–3 sentences summarizing the issue in your own words, informed by the issue body and comments.>

## Affected Files
| File | Role | Change Type |
|------|------|-------------|
| `path/to/file` | Brief description | New / Modified / Deleted |

## Implementation Steps

### 1. <First logical step>
- What to change and why.
- Which file(s) are touched.
- Key details or edge cases to handle.

### 2. <Next step>
...

### N. <Final step>
...

## Testing Strategy
- Existing tests that must keep passing: `path/to/test_file`
- New tests needed:
  - <describe test case and what it validates>

## Risks & Open Questions
- <Anything uncertain, potentially breaking, or worth discussing before starting.>

## Out of Scope
- <Anything related but intentionally excluded from this plan.>
````

### Step 5: Present and Confirm

- Display the plan in chat as a markdown code block.
- Do **not** create any files.
- After presenting the plan, ask the user:
  > "Want me to start implementing this plan?"
- If the user confirms, begin executing the steps in order, running tests after each meaningful change.
- If the user wants changes to the plan, revise and re-present before implementing.
