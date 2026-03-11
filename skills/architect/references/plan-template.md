# [Ticket-ID]: [Feature Name]

## What We're Building
One paragraph summary. What the feature does, the approach we chose, and why.
Reference the rejected alternative briefly so future-you remembers the tradeoff.

## Files to Modify (in order)

### 1. [full/path/to/file.rb]
**What changes:** Plain language description of what to add or modify.
**Why:** Brief explanation connecting this change to the feature goal.
**Pattern to follow:** Path to an existing file in the codebase that does something similar.
**Logic:** Pseudocode description in English.
  - "Add a scope that filters records by the relevant status field, ordered by the default sort column"
  - "Include the new fields in the serializer, pulling values from the related record's attributes"

### 2. [full/path/to/next_file.tsx]
...

## Testing Approach
- What to test at each layer (unit tests for business logic, integration tests for API boundaries, avoid E2E for unit-level concerns)
- Which factories or fixtures to use or create
- Edge cases identified during architecture
- What NOT to test (framework behavior, gem internals)

## PR Strategy
- Single PR or split? If split, what goes in each PR and in what order
- Migration considerations
- Feature flag needs

## Open Questions
- Anything unresolved that might come up during implementation
