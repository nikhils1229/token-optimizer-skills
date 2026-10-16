---
name: token-optimizer
description: "Master Token Optimizer & Context Reducer for High-End Model Runs. Unifies caveman output compression, investigate-first targeted slicing, surgical diff patching, session context compaction, prefix-cache alignment, and asymmetric runtime delegation to slash input and output token consumption by 50-70% on Claude Sonnet/Opus, GPT-6 Astra, and Gemini Pro. Triggers on: 'token optimizer', 'token efficiency', 'caveman mode', 'minimize tokens', 'reduce token usage', 'token budget', '/token-optimizer', '/tok-opt'."
---

# Token Optimizer: Unified Token & Context Minimization Suite

The **Token Optimizer** integrates six synchronized token reduction technologies into a single operational protocol designed specifically to make high-end model runs (Claude 3.5/3.7 Sonnet, Opus 4, GPT-6 Astra, Gemini 3.1 Pro) dramatically faster, cheaper, and resistant to context degradation.

---

## When to Activate

Activate this skill automatically whenever:
- The user requests token optimization ("token optimizer", "minimize tokens", "less tokens", "reduce cost", "/tok-opt")
- High-end models (Claude Sonnet/Opus, Astra, Gemini Pro) are executing complex, multi-turn tasks
- Context length approaches 50% of the model window or multi-agent delegation loops run repeatedly
- In conjunction with `/maestro` and `paper-writer`

---

## The 6 Core Optimization Protocols

```mermaid
flowchart TD
    User["High-End Model Task"] --> TO["Token Optimizer Master Engine"]
    
    subgraph P1 ["1. Output Terseness (-65% Output Tokens)"]
        C1["caveman: Drop filler, articles, pleasantries, tool narration"]
        C2["Density without information loss: Code, errors & numbers exact"]
    end
    
    subgraph P2 ["2. Input Slicing (-50% Input Tokens)"]
        I1["investigate-first: Targeted ripgrep over full file dumping"]
        I2["Bounded slicing: view_file with exact StartLine & EndLine"]
    end
    
    subgraph P3 ["3. Surgical Code Diffs (-80% Code Tokens)"]
        S1["surgical-patch: 1-15 line diffs via replace_file_content"]
        S2["Strict ban on full-file rewriting for minor fixes"]
    end
    
    subgraph P4 ["4. Session Compaction (-60% Multi-Turn Bloat)"]
        M1["paper-session-compaction & memory ledgering"]
        M2["Hash & summarize historical turns; purge stale stdout"]
    end
    
    subgraph P5 ["5. Prefix Cache Alignment (>90% Cache Hits)"]
        PC1["Keep invariant system prompts & tools static"]
        PC2["Reuse persistent conversation handles (--resume / --conversation)"]
    end
    
    subgraph P6 ["6. Asymmetric Runtime Delegation"]
        R1["Gemini Flash / Grep: Bulk scanning & execution"]
        R2["Claude Sonnet / Astra: Lean, high-value reasoning only"]
    end
    
    TO --> P1
    TO --> P2
    TO --> P3
    TO --> P4
    TO --> P5
    TO --> P6
    TO --> Result["Fast, Cost-Efficient, Zero-Waste Execution"]
```

---

### Protocol 1: Terse Caveman Density & Review Format (Output Compression: -65%)
*Powered by `caveman`, `caveman-compress` & `caveman-review`*
- **Drop**: Articles (*a/an/the*), conversational filler (*just/really/basically/actually/simply*), pleasantries (*sure/certainly/happy to*), hedging, and repetitive preambles.
- **Never Drop**: Logical operators (*not/never/no/only/except*), exact variable names, line numbers, file paths, and code snippets.
- **Terse Findings & Review Format**: Exactly one line per finding without throat-clearing:
  `L<line>: <severity> <problem>. <concrete fix>.` (or `<file>:L<line>: ...` for multi-file diffs).
- **Severity Prefixes**:
  - `🔴 bug:` — broken behavior, runtime exception, incident-causing defect.
  - `🟡 risk:` — fragile code (race condition, missing null check, swallowed error, unhandled edge case).
  - `🔵 nit:` — style, naming, micro-optimization. Author can ignore.
  - `❓ q:` — genuine question requiring user clarification.
- **Tool calls**: Fire directly. Never output conversational narration before or after a tool call unless explaining an irreversible or security-critical action.

### Protocol 2: Input Context Slicing & AST Routing (Input Reduction: -50% to -70%)
*Powered by `investigate-first` & `graphify`*
- **Never** call `view_file` on an entire 500+ line file to find one function.
- **AST Knowledge Graph First**: When navigating dependencies across files, run `graphify query "<symbol>"` to pinpoint callers, callees, and exact line numbers. Refresh the graph locally via `graphify extract . --code-only` after structural modifications.
- **Anti-Bloat Guardrail**: STRICT BAN on viewing, catting, or dumping `graphify-out/graph.json` or `GRAPH_REPORT.md` whole into context (prevents 10k–50k token context flooding). Only query targeted symbols.
- Always run `grep_search` or `find_by_name` first to obtain exact line ranges.
- Slice `view_file` strictly to the relevant 20–60 lines (`StartLine` and `EndLine`).
- When running commands that produce huge output (e.g. `git log`, `test`), cap output or pipe through `head -n 20` or `tail -n 20`.

### Protocol 3: Surgical Diff Patching & Minimal Code (Code Reduction: -80%)
*Powered by `surgical-patch` & `ponytail`*
- **The Ponytail YAGNI Ladder**:
  1. *Existence Check*: Does this code, wrapper, or feature need to exist? If not, skip it.
  2. *Standard Library First*: Reach for built-in language/runtime tools before installing packages.
  3. *Native Platform First*: Reach for native platform/language features before third-party wrappers.
  4. *Reuse Existing Code*: Leverage abstractions already in the project before creating new ones.
  5. *Minimal Implementation*: Write the absolute minimum lines of code required.
- **Anti-Bloat Guardrail**: Apply Ponytail SILENTLY in code. STRICT BAN on explaining or narrating the ladder steps in chat (prevents output token ballooning).
- **Strictly prohibit** overwriting entire existing files with `write_to_file` when fixing bugs.
- Always use `replace_file_content` targeting the minimal contiguous block (1–15 lines).
- Every patch must trace directly to a verified `🔴 bug:` or `🟡 risk:`.
- 100% Safety Guarantee: Maintain all validation boundaries, error handling, security checks, and invariants.

### Protocol 4: Session Compaction & Digesting
*Powered by `session-management` & `paper-session-compaction`*
- In conversations spanning more than 10 turns, compact previous turns into dense, telegraphic session state files (`docs/maestro/state/digest.md`).
- Discard raw intermediate terminal scrollbacks, keeping only final outcomes, return codes, and verified invariants.

### Protocol 5: Prompt Prefix Cache Optimization
- Ensure invariant context (system rules, tool schemas, project metadata) remains bit-identical at the start of prompts to leverage Anthropic and Google prefix caching (up to 90% discount on cached input tokens).
- Maintain persistent session IDs (`--conversation` in Gemini, `--resume` in Claude Code) across phases to hit cached memory.

### Protocol 6: Asymmetric Runtime Routing
- Route high-volume, repetitive, or bulk operations (file crawling, large regex grep, simulation runs, test suite executions) to **Gemini Flash** or local shell commands.
- Dispatch only dense, high-signal packets to high-end models (**Claude Sonnet/Opus**, **GPT-6 Astra**) for design decisions, mathematical verification, and code review.
