# Token Optimizer Skills

Bundle of 4 Claude/Gemini/Antigravity-compatible **skills** for cutting input and output token spend on long coding sessions, plus 2 slash commands that activate them. Drop `skills/` into any agent's skills directory (e.g. `~/.claude/skills/`, `~/.gemini/config/skills/`) and the matching `commands/` files into `~/.claude/commands/` to use it with Claude Code.

## What's inside

| Skill | Purpose | Triggers on |
|---|---|---|
| **`token-optimizer`** | Master skill. Unifies the other three plus session-compaction, prompt-cache alignment, and asymmetric model routing into one protocol. | `/token-optimizer`, `/tok-opt`, "minimize tokens", "caveman mode" |
| **`investigate-first`** | Forces evidence-gathering before edits on ambiguous bugs — separates observed symptom from inferred cause, ranks hypotheses, stops only once one mechanism is proven. | Unknown-cause bugs, intermittent failures, perf regressions |
| **`surgical-patch`** | Fixes bugs at the narrowest responsible layer. Bans full-file rewrites and drive-by cleanup; only 1–15 line diffs traced directly to a proven cause. | Any bugfix once root cause is known |
| **`karpathy-coder`** | Enforces Andrej Karpathy's 4 LLM-coding principles (think before coding, simplicity first, surgical changes, goal-driven execution) with Python scripts that actually detect violations (complexity, unstated assumptions, scope creep, unverified goals). | "review my diff", "am I overcomplicating this", pre-commit |

## Why this exists

High-end models (Claude Sonnet/Opus, Gemini Pro) burn tokens three ways during agentic coding sessions:

1. **Verbose output** — pleasantries, restated plans, narrated tool calls.
2. **Unbounded input** — dumping whole files into context to find one function.
3. **Full-file rewrites** — regenerating 300 lines to fix 3.

Each skill below attacks one of these directly instead of relying on "please be concise" prompting, which degrades over long sessions.

## The 6 protocols (`token-optimizer`)

1. **Output terseness** — caveman-style compression: drop articles/filler/pleasantries, keep code/numbers/logic exact. One-line finding format: `<file>:L<line>: <severity> <problem>. <fix>.`
2. **Input slicing** — grep/AST-locate before reading; slice file reads to the 20–60 relevant lines, never whole files.
3. **Surgical diffs** — 1–15 line patches only, traced to a proven bug; no full-file `write` for small fixes.
4. **Session compaction** — compress old turns into dense digests past ~10 turns; discard raw scrollback, keep outcomes.
5. **Prefix-cache alignment** — keep system prompt/tool schemas byte-identical across turns to hit provider prompt caching (up to 90% discount on cached input).
6. **Asymmetric routing** — bulk/repetitive work to cheap/fast models, high-value reasoning to the frontier model.

Claimed savings (author's informal benchmarking, not independently verified): ~50–70% combined input+output token reduction on multi-turn coding sessions.

## Install

### Claude Code
```bash
git clone https://github.com/<your-username>/token-optimizer-skills.git
cp -R token-optimizer-skills/skills/* ~/.claude/skills/
cp token-optimizer-skills/commands/*.md ~/.claude/commands/
```

### Gemini CLI / Antigravity
```bash
cp -R token-optimizer-skills/skills/* ~/.gemini/config/skills/
```

Each skill is self-contained (its own `SKILL.md` with YAML frontmatter `name`/`description`); copy only the subdirectories you want.

## Usage

- `/token-optimizer` or `/tok-opt` — turn the full protocol on for the session (stays on until `/token-optimizer off` or "normal mode").
- Say "investigate this bug" / mention an intermittent or unexplained failure — `investigate-first` engages automatically.
- Any bugfix once cause is known — `surgical-patch` keeps the diff minimal.
- "review my diff", "am I overcomplicating this", before a commit — `karpathy-coder` runs its checks (`scripts/complexity_checker.py`, `scripts/assumption_linter.py`, `scripts/diff_surgeon.py`, `scripts/goal_verifier.py`).

## Repo layout

```
token-optimizer-skills/
├── skills/
│   ├── token-optimizer/SKILL.md
│   ├── investigate-first/{SKILL.md, agents/openai.yaml}
│   ├── surgical-patch/{SKILL.md, agents/openai.yaml}
│   └── karpathy-coder/{SKILL.md, references/*.md, scripts/*.py}
├── commands/
│   ├── token-optimizer.md
│   └── tok-opt.md
├── LICENSE
└── README.md
```

## License

MIT — see [LICENSE](LICENSE). `skills/karpathy-coder/` is a third-party MIT-licensed skill included unmodified; attribution preserved in its `SKILL.md` frontmatter and in LICENSE.
