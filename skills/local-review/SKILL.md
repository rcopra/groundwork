---
name: local-review
description: >
  This skill should be used when the user says "review my code", "check my work",
  "ready for review", "did I miss anything", "pre-PR check", or "is this ready".
  Also trigger when implementation is complete and the user wants verification before
  opening a PR.
---

# Local Review

Review the developer's branch diff against main for correctness and completeness. This is a pre-PR sanity check — find bugs and gaps, not style preferences. If the code works and covers the requirements, say so and get out of the way.

## When Activated

Announce yourself:

> **Local Review** — I'll check your diff against main for bugs, missing pieces, and test coverage. No style opinions — just correctness.

## Context Independence

This skill reads everything it needs from disk: the git diff and the plan file. It does not depend on prior conversation context from ticket-analysis, architect, or implementation-guide. It works equally well in a fresh session or a long-running one.

## Phases

### Phase 1: Gather Context

1. **Get the diff.** Run `git diff main...HEAD` (substitute `master` or your primary branch name if different) to see all changes on this branch.
2. **Find the plan.** Look in `.claude/plans/` for an implementation guide matching the ticket ID (extract from branch name). If no plan exists, work from the diff alone — that's fine.
3. **Read changed files in full.** Don't review just the diff hunks — read the complete files to understand context around changes.

### Phase 2: Correctness Review

Adjust these checks to match the project's stack and conventions — the examples below skew toward Rails/React but the categories apply universally.

For each changed file, check for:

- **Logic errors** — Wrong conditionals, nil handling, off-by-one, incorrect boolean logic, missing return values
- **Missing error handling** — Failure paths that aren't handled, exceptions that could bubble up unexpectedly
- **Authorization gaps** — New endpoints or actions missing authorization checks (policy objects, middleware guards, permission checks — whatever the project uses)
- **Data integrity** — Operations that should be wrapped in transactions but aren't, race conditions on concurrent updates
- **SQL concerns** — N+1 queries (especially in loops or serializers), missing database indexes for new query patterns
- **Security** — Unsanitized input, missing authorization on new parameters, missing input validation at system boundaries
- **Frontend** — Missing loading/error states, stale cache after mutations, type mismatches between API response and component props

**Be specific.** Don't say "consider error handling here." Say "If `order.line_items` is not loaded or returns null/nil, calling `.each` on it will raise a null reference error. Add a null guard or ensure the association is eager-loaded in the query."

### Phase 3: Completeness Review

Cross-reference the diff against the implementation plan (if one exists):

- **All planned files modified?** — Flag any files listed in the plan that weren't changed
- **All AC addressed?** — Map each acceptance criterion to the code that implements it
- **No TODO/placeholder code left?** — Search for TODO, FIXME, HACK, placeholder comments
- **Edge cases from architecture covered?** — Check if edge cases discussed during planning are handled in the code

If there's no plan, review against the ticket AC directly (ask for the ticket ID if needed).

### Phase 4: Test Review

Per your project's testing guidelines:

- **Tests exist for behavioral changes?** — New business logic, new endpoints, changed behavior all need tests
- **Tests at the right level?** — Unit tests for business logic, integration tests for API endpoints. Not E2E for field validations
- **Testing behavior, not implementation?** — Tests should verify outcomes, not internal method calls or instance variables
- **Failure paths tested?** — At least one failure case per significant operation
- **No test bloat?** — Same behavior shouldn't be tested at multiple layers. Model validation tested in model spec doesn't need re-testing in request spec

### Phase 5: Report

Two categories only:

**Issues** (fix before PR):
- Bugs, logic errors, missing tests, security gaps, authorization gaps
- Each issue includes: what's wrong, where (file + line), why it matters, what to do about it

**Notes** (awareness, not blocking):
- Edge cases worth thinking about, deployment considerations, things to mention in the PR description
- These are informational — the developer decides whether to act on them

**If the code is clean:** "Implementation looks solid. All AC covered, tests are at the right level, no bugs found. Ready for PR."

Don't manufacture issues to seem thorough. A clean review is a good review.

## What This Review Does NOT Do

- Suggest renaming variables
- Suggest extracting methods or classes
- Suggest "improvements" or "enhancements" beyond the ticket scope
- Comment on code style (that's your project's linter and formatter's job)
- Suggest additional features
- Recommend refactoring nearby code
- Nitpick formatting, whitespace, or import order

The question is: **Does this code correctly implement what the ticket asks for?** Nothing more.

## Out of Scope

- Linting: Don't duplicate what automated tools catch.
- Redesigning: Don't redesign the approach at review time. If the architecture is fundamentally wrong, say so plainly, but don't suggest restructuring working code for aesthetic reasons.
- Teaching: Save the teaching moments — the developer needs a clear pass/fail, not a lecture.

## Terminal State

When the review is complete:

> **Review complete.** [Summary of findings — either issues to fix or confirmation that it's clean.] You can open a PR now.

This is the end of the workflow chain. The developer takes it from here.
