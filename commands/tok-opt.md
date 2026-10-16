---
description: Activate the unified Token Optimizer suite to slash token consumption by 50-70% across high-end models (caveman + investigate-first + surgical-patch + session-compaction)
---

# Token Optimizer Master Command

When the user runs `/token-optimizer` or `/tok-opt`, or asks to "optimize tokens", "minimize tokens", or "use caveman mode":

1. **Activate the Token Optimizer Protocol** (`~/.gemini/config/skills/token-optimizer/SKILL.md`):
   - **Output Density**: Switch to Caveman compression (drop filler, pleasantries, articles; keep logic, code, and errors exact).
   - **Input Slicing**: Bounded file viewing (never view whole files when 30 lines suffice; use grep first).
   - **Surgical Patching**: Apply 1-15 line contiguous diffs; never overwrite whole files.
   - **Session Compaction**: Compact long session histories into telegraphic digests.
   - **Prefix Cache Hit**: Retain persistent session IDs to leverage 90% prompt cache discounts.
   - **Asymmetric Tiering**: Bulk exploration on Flash; high-value reasoning on Sonnet/Opus/Astra.

2. **Persistence**:
   - Token Optimizer remains active until explicitly turned off by the user (`/token-optimizer off` or "normal mode").
