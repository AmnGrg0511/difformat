# Difformat

VS Code extension concept for semantic diffs that ignore whitespace and non-functional formatting changes. The goal is to help reviewers focus on real code changes, not layout noise.

## Highlights
- Semantic diff view that ignores spacing-only changes
- Optional formatting normalization before diffing
- Lightweight UX for toggling comparison modes

## Core Approach
- Normalize both sides with the same formatter and spacing rules, then diff the normalized output
