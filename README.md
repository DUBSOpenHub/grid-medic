# 🚑 Grid-Medic

> *Your agents just ran a mission. Who's checking if they came back healthy?*

Grid-Medic is a self-healing meta-agent that monitors, repairs, and continuously improves your AI agent fleet. It reads scan outputs, diagnoses failures, proposes minimal fixes, validates every change across multiple AI models, and auto-applies only what passes consensus.

Think of it as the immune system for your agents. They do the work. Grid-Medic keeps them healthy.

---

🌍 **Works with any agent.** Scans `.agent.md` prompt files for Copilot CLI, but the patterns apply to any AI agent framework. Zero dependencies beyond the Copilot CLI itself.

🏭 **Built with [Dark Factory](https://github.com/DUBSOpenHub/dark-factory)** — 6 AI agents, [sealed-envelope testing](https://github.com/DUBSOpenHub/shadow-score-spec), validated and shipped.

🔬 **Pairs with [Agent X-Ray](https://github.com/DUBSOpenHub/agent-xray)** — X-Ray scans your agents for weaknesses. Grid-Medic fixes what it finds.

---

## Why This Tool?

Agents degrade. APIs change, scoring goes stale, edge cases pile up, new capabilities emerge that your agents don't leverage. Manually reviewing and updating 8+ agent prompt files is tedious and error-prone.

Grid-Medic automates the entire maintenance lifecycle:

- **Self-healing** — detects and fixes API errors (403 misclassification, deprecated endpoints, malformed queries) without human intervention
- **Multi-model validation** — every change is reviewed by 3 AI models before being applied. No cowboy commits to your agent prompts
- **Full auditability** — every diagnosis, proposal, validation result, and applied change is logged with rationale
- **Fleet health tracking** — quality scores per agent, trend lines over time, a dashboard showing which agents need attention

## 🚀 Quick Start

Grid-Medic runs as a [Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli) custom agent.

### Install

```bash
# Copy the agent file to your Copilot agents directory
curl -fsSL https://raw.githubusercontent.com/DUBSOpenHub/grid-medic/main/grid-medic.agent.md \
  -o ~/.copilot/agents/grid-medic.agent.md
```

Restart your Copilot CLI session (`/exit` then `copilot`). Grid-Medic will appear in your agent list.

### Run

Open Copilot CLI and say any of these:

```
grid-medic diagnose                    # Scan all agents for issues
grid-medic improve security-audit      # Focus on one agent
grid-medic review-log                  # See improvement history
grid-medic /path/to/scan-output        # Analyze sweep results
```

That's it. Point it at your agents and let it work.

## 🔧 The 5-Phase Improvement Cycle

```
  📥 Input          🔍 Diagnose        💊 Prescribe       🧪 Validate         ✅ Apply
  ─────────── ──▶ ─────────── ──▶ ─────────── ──▶ ─────────── ──▶ ───────────
  diagnose         Read agents        Generate          Send to 3 models     Auto-apply
  improve X        Detect issues      minimal fixes     (2/3 consensus)      + log everything
  scan output      Categorize         Exact old/new     Approve / Reject     Rollback if broken
```

### Phase 1 — Diagnosis

Reads all agent files from `~/.copilot/agents/` and categorizes issues:

| Category | Icon | What It Catches |
|----------|------|-----------------|
| API Fix | 🔴 | API calls returning errors, deprecated endpoints, malformed commands |
| Logic Fix | 🔴 | Missing fallback logic, edge cases that crash the agent |
| Scoring | 🟡 | Formulas that produce unintuitive or unfair results |
| Output | 🟡 | Missing evidence citations, broken formatting |
| Prompt Clarity | 🟡 | Ambiguous instructions causing inconsistent behavior |
| Performance | 🟡 | Redundant API calls, sequential ops that could be parallel |
| New Dimension | 🟢 | New data sources or analysis dimensions to add |

### Phase 2 — Prescription

For each issue, Grid-Medic generates a specific, minimal improvement with exact old/new text. No rewrites. Surgical edits only.

### Phase 3 — Multi-Model Validation

Every proposed change gets sent to 3 AI models in parallel:

```
  💊 Proposed Change
      │
      ├──▶ Claude Sonnet    ──▶ ✅ 8/10
      ├──▶ GPT Codex        ──▶ ✅ 7/10
      └──▶ Gemini Pro       ──▶ ❌ 4/10
                                    │
                            2/3 = Apply with note
```

| Consensus | Action |
|-----------|--------|
| 3/3 Approve | Auto-apply immediately |
| 2/3 Approve | Apply with "majority approved" note |
| 1/3 Approve | Log as proposed, don't apply |
| 0/3 Approve | Reject and log reason |

### Phase 4 — Application

Approved changes are applied to agent files with automatic rollback if anything breaks. YAML frontmatter is validated after every edit.

### Phase 5 — Logging

Everything is logged to `~/.copilot/grid-medic-log.md` and tracked in SQL for cross-session trend analysis.

## 📊 Sample Output

```
🚑 Grid-Medic Report
══════════════════════════════════════════════════════════

📋 DIAGNOSIS
  Agents scanned:     8
  🔴 Errors:          1
  🟡 Inefficiencies:  2
  🟢 Enhancements:    1

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💊 IMPROVEMENTS

  [🔴] security-audit: Fix code-scanning 403 response handling
    Validation: {Sonnet: ✅ 9/10} {Codex: ✅ 8/10} {Gemini: ✅ 9/10} → APPLIED ✅

  [🟡] msft-impact: Add path: filter to org:microsoft code search
    Validation: {Sonnet: ✅ 8/10} {Codex: ✅ 7/10} {Gemini: ✅ 8/10} → APPLIED ✅

  [🟡] octoscanner: Add retry logic for stats/commit_activity 202
    Validation: {Sonnet: ✅ 9/10} {Codex: ✅ 8/10} {Gemini: ✅ 7/10} → APPLIED ✅

  [🟢] compliance-inspector: Add SPDX license list version check
    Validation: {Sonnet: ✅ 6/10} {Codex: ❌ 4/10} {Gemini: ✅ 5/10} → PROPOSED

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 AGENT HEALTH DASHBOARD
  Agent                  Quality  Issues  Trend
  ──────────────────────────────────────────────
  repo-detective         9/10     0       → Stable
  security-audit         8/10     0       ↑ Fixed (was 6/10)
  contact-info           9/10     0       → Stable
  social-presence        8/10     0       → Stable
  msft-impact            8/10     0       ↑ Fixed (was 6/10)
  compliance-inspector   8/10     1       → Stable
  full-sweep             8/10     0       → Stable
  octoscanner            8/10     0       ↑ Fixed (was 7/10)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📈 CUMULATIVE STATS
  Total improvements applied:  3
  Total improvements proposed: 1
  Fleet quality average:       8.3/10

🚑 Grid-Medic signing off.
```

## 🛡️ Safety Rules

- **Never break a working agent.** If uncertain, propose but don't apply.
- **Validate everything.** No change is applied without multi-model consensus (≥ 2/3).
- **Log everything.** Every diagnosis, proposal, validation, and application is recorded.
- **Minimal changes.** Surgical edits only. Never rewrite an entire agent file.
- **Preserve personality.** Agent codenames, emojis, and voice are sacred.
- **Evidence-based.** Improvements must be justified by observed failures.
- **Rollback on failure.** If a change breaks the file, immediately revert.
- If no issues are found, say so and sign off. Don't invent problems.

## 🔬 Works Great With

| Tool | How They Work Together |
|------|----------------------|
| [Agent X-Ray](https://github.com/DUBSOpenHub/agent-xray) | X-Ray scans for weaknesses. Grid-Medic fixes them. |
| [Groundhog Day](https://github.com/DUBSOpenHub/groundhog-day) | Groundhog backs up your skills. Grid-Medic keeps agents healthy. |
| [Dark Factory](https://github.com/DUBSOpenHub/dark-factory) | Dark Factory builds agents. Grid-Medic maintains them post-deploy. |

## License

MIT

---

🐙 Created with 💜 by [@DUBSOpenHub](https://github.com/DUBSOpenHub) with the [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli).

Let's build! 🚀✨
