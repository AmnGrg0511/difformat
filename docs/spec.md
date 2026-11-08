---
tags: [flagship, vscode-extension, developer-tools, vcs, diff, git, perforce]
aliases: [Difformat, Semantic Diff, Format-Then-Diff, Code Review]
created: 2026-06-26
status: solid
---

# Difformat — Semantic Diff for Code Review

> **Status:** ✅ Solid spec. Ready to build.
> **Platform:** VS Code extension
> **Stack:** TypeScript + VS Code API + tree-sitter
> **Effort:** ~34 hours

---

## The Story

You review a PR. The actual change is 5 lines. But the diff shows 200 lines because someone ran clang-format on the legacy codebase. You can't see the signal for the noise.

**Difformat solves this.** It formats both sides of a diff with the same rules, THEN diffs. Formatting-only changes disappear. You see only the real changes.

**The niche:** C++ support. SemanticDiff (the competitor) supports Python, Rust, Java, TypeScript — but NOT C++. Difformat starts with C++.

**Inspired by:**
- SemanticDiff (language-aware diff, but no C++)
- difftastic (structural diff, tree-sitter based)
- clang-format (C++ formatter)

---

## The Problem

**Code review is broken when formatting changes mix with logic changes:**
- Legacy codebase cleanup creates massive diffs
- Reviewers miss real changes in formatting noise
- `git diff --ignore-space-change` is too crude
- No tool does "format-then-diff" for C++

**Why C++ matters:**
- EDA companies (like PowerPro) use C++ heavily
- C++ formatting is complex (templates, macros, preprocessor)
- SemanticDiff doesn't support C++
- clang-format is the standard — Difformat uses it

---

## The Solution

**Difformat = Format both sides → Diff → Show only real changes**

1. **Format-then-diff** — Apply same formatter to both sides, then diff
2. **C++ first** — clang-format integration, tree-sitter-cpp for parsing
3. **VCS-agnostic** — Works with Git, Perforce, any SCM via VS Code API
4. **Native diff view** — Uses VS Code's built-in diff viewer (no webview)

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    DIFORMAT                                  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  VS CODE UI                                            │ │
│  │  - Command: "Difformat: Compare Files"                 │ │
│  │  - Command: "Difformat: Compare with Git Ref"          │ │
│  │  - Native diff view (vscode.diff)                      │ │
│  └──────────────────────┬─────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────┴─────────────────────────────────┐ │
│  │  EXTENSION CORE (TypeScript)                           │ │
│  │  - Formatter abstraction layer                         │ │
│  │  - Diff engine (two-pass)                              │ │
│  │  - SCM integration (Git, Perforce)                     │ │
│  └──────────────────────┬─────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────┴─────────────────────────────────┐ │
│  │  FORMATTERS                                            │ │
│  │  - C++: clang-format (CLI)                             │ │
│  │  - JS/TS: Prettier                                     │ │
│  │  - Python: Black                                       │ │
│  │  - Rust: rustfmt                                       │ │
│  └──────────────────────────────────────────────────────  │ │
└─────────────────────────────────────────────────────────────┘
```

---

## Features

### P0 — MVP (~20h)

1. **C++ format-then-diff** — clang-format both sides, diff
2. **Git integration** — Compare working file vs any ref
3. **Native diff view** — Uses VS Code's built-in diff
4. **Configurable** — .difformat.json for formatter rules

### P1 — Multi-language (~10h)

5. **JavaScript/TypeScript** — Prettier integration
6. **Python** — Black integration
7. **Rust** — rustfmt integration

### P2 — VCS-agnostic (~4h)

8. **Perforce integration** — Compare with depot revision
9. **Any SCM** — Generic SCM provider interface

---

## Key Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| Formatter | clang-format CLI | Most reliable for C++ |
| Diff algorithm | Line diff with word highlighting | Simple, fast |
| UI | Native VS Code diff | No webview, clean |
| Language support | C++ first | SemanticDiff gap |
| Bundled formaters | No — user installs | Avoid bloat |

---

## Monetization

| Tier | Price | What |
|------|-------|------|
| **Free** | $0 | All features, all languages |
| **Sponsor** | $5/mo | Support development, early access |

**Open source, MIT license.** Monetization via sponsorship, not features.

---

## Worth Assessment

| Dimension | Score | Notes |
|-----------|-------|-------|
| Portfolio value | 8/10 | VS Code extension + C++ niche |
| Learning value | 8/10 | VS Code API, tree-sitter, formatters |
| Market/user value | 7/10 | C++ developers need this |
| Personal value | 8/10 | You'll use it at PowerPro |

---

## Dependencies

- **None** — Standalone extension

---

## Quality Markers

- [ ] C++ format-then-diff works
- [ ] Git integration (compare with any ref)
- [ ] Native VS Code diff view
- [ ] Configurable via .difformat.json
- [ ] Perforce integration
- [ ] Works with clang-format, Prettier, Black, rustfmt

---

**Build third.** No dependencies. C++ niche is unique.
