# Improvicus Log

Read this file before running Agenticus Improvicus. Append only useful run summaries and prompt improvements.

## 2026-11-01 - Syncer Prompt Refinement
- **Target:** `agents/syncer.md`
- **What was missing:**
  - `syncer.md` had **0** `rg` commands in the entire file; all detection was abstract prose or legacy `grep` loops.
  - Step 2 lacked diagnostic commands for detecting markdown anti-patterns (unbalanced code fences, duplicate headings, stale root log paths, thin handoff tables, missing required sections).
  - Lacked concrete before/after examples of additive section merges, handoff table updates, log path migrations, and project customization conflict preservation.
  - Verification section lacked explicit exit code checks, fence balancing audits, and git status validation.
  - Industry Standards was thin (4 bullets), lacking specific API details (GitHub rate limits, Python `json` parsing without `jq`, `rg` version flags).
- **Improvements applied:**
  - Added 27 concrete, executable `rg` commands across Step 2 and Step 4 with proper flags (`--glob`, `-n`, `-c`, `-q`).
  - Added 4 robust before/after code examples: adding missing Senior Engineering Standards, appending rows to Cross-Domain Handoff, fixing stale log path references, and resolving conflict by preserving project-specific boundaries.
  - Upgraded Step 2 problem detection with automated scans for unbalanced code fences, duplicate headings, stale upstream URLs, and thin handoff tables.
  - Expanded Step 4 verification with 8 discrete checks including fence pairing tests, exit status handling, and diff inspection.
  - Enhanced Industry Standards with Context7 citations (`docs.github.com/en/rest`, `BurntSushi/ripgrep`), GitHub API rate-limit realities, and Python parsing idioms.
  - Added 2 new Senior Engineering Standards bullets regarding `rg` usage and evaluating line-count deltas safely.
- **Verification:** Markdown code fences balanced (38 markers, 19 pairs ✅), all 5 required steps present ✅, all required sections present ✅, `git diff --check` clean ✅.
- **Line count:** 362 → 627 (+265 lines net).

## 2026-11-01 - Refactorer Prompt Refinement
- **Target:** `agents/refactorer.md`
- **What was missing:**
  - God-object detection covered only TypeScript and Python; Go, Java, and Rust were absent.
  - Parameter-overload detection had no glob exclusions and covered only 3 languages (missing JS arrow functions, Java, Rust).
  - Magic number/string detection missing Python single-quoted strings, Go case strings, and Rust match arms.
  - Deep-nesting detection missing Go (tab-based), Java, Python nested control flow, and callback-chain patterns.
  - Duplicate-code detection relied on a fragile shell loop over function names; no per-language `rg` approach for Go/Rust/Python.
  - Switch-on-type anti-pattern (Open/Closed violation) had no detection commands at all.
  - Before/after examples existed only for TypeScript (4 examples); Python, Go, and Rust had zero examples.
  - Industry Standards block had 4 thin bullets with 2 citation URLs; no pinned version guidance, no language-specific idioms.
  - Senior Engineering Standards missing the polymorphism alternative wording and cyclomatic-complexity tooling guidance.
- **Improvements applied:**
  - Extended God-object detection to 5 languages (TS, Python, Go, Java, Rust) with proper glob exclusions (`vendor`, `target`, `build`, `generated`).
  - Rewrote parameter-overload detection with language-specific `rg` patterns for JS arrow functions, Python, Go, Java, Rust.
  - Replaced 2-line magic-values block with 5-language detection (TS/JS, Python, Go case strings, Rust match arms) plus uniq-count heuristics.
  - Rewrote deep-nesting block with tab-aware Go detection, Java spacing threshold, Python control-flow check, and callback-chain pattern.
  - Replaced fragile shell-loop duplicate-detection with per-language `rg` approaches and a dedicated Switch-on-Type sub-section with detection commands for TS, JS, Python, and Java.
  - Added 6 new before/after examples: Python extract-long-function, Python Protocol dispatch, Go guard-clause flattening, Go named-constants extraction, Rust enum dispatch (replacing match-on-string).
  - Expanded Industry Standards to 10 bullets covering Python 3.12+ Protocol, Go `%w` error wrapping, Rust enum exhaustiveness, Java 21 records, and cyclomatic-complexity tooling.
  - Added 2 new Senior Engineering Standards bullets: polymorphism alternative for switch-on-enum, and cyclomatic-complexity measurement guidance with language-specific tools.
- **Verification:** Markdown code fences balanced (40 markers, 20 pairs ✓), all 5 steps present ✓, Industry Standards section present ✓, Boundaries and Safety present ✓, rg command count raised from 11 → 34.
- **Line count:** 407 → 751 (+344 lines net).

## 2026-10-07 - Bolt Prompt Refinement
- **Target:** `agents/bolt.md`
- **What was missing:**
  - Missing Context7 Industry Standards block.
  - Missing dedicated log-first workflow (`./BOLT_LOG.md`) and Start Here protocol.
  - Missing structured run log format.
- **Improvements applied:**
  - Injected `## Start Here (Log-First Protocol)` establishing `./BOLT_LOG.md` review before code inspection.
  - Added `## Industry Standards (verified 2026-10 via Context7)` covering Cloudflare D1 statement batching, Workers edge caching, React 19 SSR tree-shaking, Compose UI memoization, and pooled backend I/O.
  - Added standard `## Log Format` for appending entries to `./BOLT_LOG.md`.
- **Verification:** All markdown code fences balanced, all 5 steps present, Context7 section verified, Boundaries and Safety preserved.

## 2026-11-01 - Linter Agent Diagnostic Coverage Expansion

- **Target:** `agents/linter.md`
- **Selection rationale:** `linter.md` had the lowest `rg` command count (4) of all 26 agents — far below the catalog median of 20+. Critically, Step 2 ("Find Problems") had zero grep-based fallbacks for when no linter is configured yet. Every other detection signal relied on tool-native CLI commands (ESLint, Prettier, ruff, gofmt) that fail silently in new projects or bare environments. The agent was effectively blind in the most common situation where it is most needed.
- **What was missing:**
  1. No `rg` diagnostics for Python/Go/Rust naming violations
  2. No detection of magic string literals and hardcoded URLs/ports
  3. No quote style inconsistency scan (mixed single/double, f-string vs %-format)
  4. No TODO/FIXME leakage detection with ticket-reference enforcement
  5. No inline disable comment audit (`eslint-disable`, `@ts-ignore`, `# noqa`, `//nolint`)
  6. No trailing-whitespace grep fallback (only relied on .editorconfig tooling)
  7. Before/after fix examples only for JS/TS boolean naming and console.log — no Go, Rust, Python, or import-ordering examples
  8. Industry Standards cited only 3 sources with 4 generic bullets; no version-specific guidance
- **Improvements applied:**
  1. Added 5 new Step 2 diagnostic subsections with 20+ concrete `rg` commands (rg count: 4 → 24)
  2. Added naming convention checks for Python (snake_case functions, PascalCase classes), Go (exported PascalCase), Rust (pub fn snake_case, type PascalCase)
  3. Added magic string detection: hardcoded localhost URLs, raw port integers, repeated string literals (3+ occurrences)
  4. Added quote style inconsistency scan for JS/TS (mixed single/double) and Python (f-string vs %-format)
  5. Added TODO/FIXME leakage scan with ticket-reference enforcement pattern
  6. Added inline disable comment audit across TS (`eslint-disable`, `@ts-ignore`), Python (`# noqa`), Go (`//nolint`)
  7. Added trailing-whitespace grep fallback as tool-agnostic safety net
  8. Expanded fix examples from 2 to 8: added TypeScript import ordering, magic string → constant, Python naming, Go gofmt/naming, Rust naming, inline disable annotation
  9. Upgraded Industry Standards: 4 sources → 6 sources, 4 bullets → 10 bullets, added ESLint 9 flat config migration note, typescript-eslint v8 setup, Prettier 3 defaults, Ruff vs flake8/black conflict warning, gofmt non-configurability rationale, rustfmt workspace usage
- **Verification:**
  - Markdown code fences: balanced (62 markers — 31 pairs) ✅
  - All 5 required steps present ✅
  - Industry Standards section present ✅
  - Boundaries and Safety sections present ✅
  - Log-First Protocol present ✅
  - `git diff --check`: clean (no trailing whitespace) ✅
- **Line count:** 480 → 717 (+237 lines net, 244 inserted, 7 deleted)

## 2026-11-01 - Docs Prompt Elevation
- **Target:** `agents/docs.md`
- **What was missing:**
  - Step 3 fix examples covered only TypeScript + Python; Go, Rust, PHP/Java all had Step 2 detection but no paired fix pattern.
  - No fix pattern for `.env.example` gaps (Step 2 detected undocumented env vars but Step 3 left agents without a concrete template to follow).
  - No CHANGELOG repair pattern despite Step 2 detecting malformed entries.
  - Industry Standards block was 4 thin bullets with no direct URLs and missing Rust/Go/PHP/Java/CHANGELOG citation anchors.
  - Step 2 had no detection for Rust doc-test `# Examples` coverage or `.env.example` key diffing using ripgrep.
- **Improvements applied:**
  1. **New fix pattern — Go doc comment:** realistic before/after for an exported generic function showing Go doc conventions (`go doc`, first sentence starts with the name, runnable `Example` in doc comment).
  2. **New fix pattern — Rust rustdoc:** before/after for a public `retry` function with `/// `, `# Arguments`, `# Errors`, `# Examples` doc-test block, and a note to run `cargo test --doc`.
  3. **New fix pattern — `.env.example` documentation:** before/after showing an incomplete `.env.example` → a complete one with every key, safe placeholder, and inline comment for value format.
  4. **New fix pattern — CHANGELOG repair:** before/after converting a bare version header to Keep-a-Changelog 1.0.0 format with ISO date, typed sections (Added/Fixed/Removed), and version diff links.
  5. **New Step 2 detection sections — Undocumented Environment Variables and CHANGELOG Integrity:** ripgrep commands to diff `process.env.*` / `os.environ` references against `.env.example`, detect missing version links, and scan Rust doc-tests for missing `# Examples` blocks.
  6. **Expanded Industry Standards block:** 4 bullets → 11 with direct hyperlinked sources for Go (`pkg.go.dev`), Rust (`doc.rust-lang.org/rustdoc`), Python (Google style guide), PHP (`phpdoc.org`), Java (Oracle Javadoc), Keep-a-Changelog, and Conventional Commits.
- **Verification:** Code fences balanced (40/40), all 5 steps present, Industry Standards ✓, Boundaries ✓, Safety ✓. Line count: 505 → 734.
