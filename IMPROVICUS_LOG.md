# Improvicus Log

Read this file before running Agenticus Improvicus. Append only useful run summaries and prompt improvements.

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
