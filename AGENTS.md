# Agents

## Overview

Grid-Medic is a single-agent self-healing system for AI agent fleets. It reads `.agent.md` prompt files, diagnoses issues, proposes minimal targeted fixes, validates every change across multiple AI models (requiring 2/3 consensus), and auto-applies only what passes. Every action is logged with rationale for full auditability.

## Available Agents

### grid-medic

- **Purpose**: Monitors, diagnoses, repairs, and continuously improves AI agent prompt files. Detects API errors, logic gaps, scoring issues, output problems, and prompt ambiguity. Runs a 5-phase improvement cycle: Input → Diagnose → Prescribe → Validate → Apply.
- **Usage**: Install `grid-medic.agent.md` to `~/.copilot/agents/`, restart Copilot CLI, then invoke with natural language:
  ```
  grid-medic diagnose                  # Scan all agents for issues
  grid-medic improve <agent-name>      # Focus repair on one agent
  grid-medic review-log                # View improvement history
  grid-medic /path/to/scan-output      # Analyze sweep results from another tool
  ```
- **Model**: Default model configured in your Copilot CLI session
- **Install**:
  ```bash
  curl -fsSL https://raw.githubusercontent.com/DUBSOpenHub/grid-medic/main/grid-medic.agent.md \
    -o ~/.copilot/agents/grid-medic.agent.md
  ```

## Issue Categories

| Category | Severity | What It Catches |
|----------|----------|-----------------|
| API Fix | 🔴 Critical | Errors, deprecated endpoints, malformed commands |
| Logic Fix | 🔴 Critical | Missing fallbacks, crash-causing edge cases |
| Scoring | 🟡 Warning | Unfair or unintuitive scoring formulas |
| Output | 🟡 Warning | Missing citations, broken formatting |
| Prompt Clarity | 🟡 Warning | Ambiguous instructions causing inconsistency |
| Performance | 🟡 Warning | Redundant API calls, sequential ops that could be parallel |
| New Dimension | 🟢 Enhancement | New data sources or analysis capabilities |

## Configuration

- Agent files are read from `~/.copilot/agents/` by default
- Multi-model validation requires 2/3 model consensus before any change is applied
- All changes are logged with full rationale; rollback is automatic if a fix breaks behavior
- Works with any `.agent.md` files — not limited to Grid-Medic's own agents
- Pairs with [Agent X-Ray](https://github.com/DUBSOpenHub/agent-xray) for deeper weakness scanning
