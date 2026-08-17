# Agent Skills for LangChain, LangGraph, LangSmith & Deep Agents

A comprehensive collection of agent-optimized skills for AI coding assistants working in the LangChain ecosystem. These skills cover the complete development lifecycle from project setup to production deployment, monitoring, debugging, and Deep Agents setup/configuration.

Each skill is a self-contained package with a `SKILL.md` entry point plus optional scripts, references, and templates.

## Available Skills

| Skill | Description |
| --- | --- |
| [langgraph-project-setup](./skills/langgraph-project-setup/) | Initialize and configure LangGraph projects (structure, `langgraph.json`, env vars, dependencies). |
| [langgraph-agent-patterns](./skills/langgraph-agent-patterns/) | Multi-agent coordination patterns: supervisor, router, orchestrator-worker, handoffs. |
| [langgraph-state-management](./skills/langgraph-state-management/) | State schemas, reducers, persistence, checkpoint inspection, and migration workflows. |
| [langgraph-error-handling](./skills/langgraph-error-handling/) | Retry strategies, LLM-based recovery loops, and human-in-the-loop escalation patterns. |
| [langgraph-testing-evaluation](./skills/langgraph-testing-evaluation/) | Test and evaluate LangGraph agents with unit/integration patterns, trajectory evaluation, LangSmith dataset evals, and A/B comparisons. |
| [langsmith-trace-analyzer](./skills/langsmith-trace-analyzer/) | Fetch, organize, and analyze LangSmith traces for debugging and performance optimization. |
| [langsmith-deployment](./skills/langsmith-deployment/) | Deploy, monitor, and manage LangGraph applications in production (Cloud, Hybrid, Standalone). |
| [deepagents-setup-configuration](./skills/deepagents-setup-configuration/) | Initialize, configure, and validate Deep Agents projects (Python/JavaScript), including middleware, backends, subagents, persistence, and migration guidance. |
| [deepagents-planning-todos](./skills/deepagents-planning-todos/) | Master the `write_todos` tool for task planning and decomposition in Deep Agents, with patterns for research, coding, analysis tasks, and trace visualization. |
| [skill-creator](./skills/skill-creator/) | Guidance for creating and maintaining skills in this repo. |

## Coverage

**9 production-ready skills** covering the complete LangChain/LangGraph/Deep Agents development lifecycle:

- 🚀 **Project Setup** - Initialize projects with proper structure and configuration
- 🤝 **Multi-Agent Patterns** - Supervisor, router, orchestrator, and handoff patterns
- 💾 **State Management** - Schemas, reducers, persistence, and checkpointing
- 🛡️ **Error Handling** - Retry policies, LLM recovery, and human-in-the-loop
- ✅ **Testing & Evaluation** - Unit/integration tests, trajectory evaluation, LangSmith evaluation, A/B testing
- 🔍 **Trace Analysis** - Debug with LangSmith traces and pattern detection
- 🌐 **Production Deployment** - Cloud, Hybrid, and Standalone deployment with monitoring
- 🧠 **Deep Agents Setup** - Initialize and configure Deep Agents with validated project scaffolding and compatibility guidance
- 📝 **Deep Agents Planning** - Master write_todos for effective task decomposition with patterns, best practices, and trace visualization

Plus the **skill-creator** meta-skill for extending this collection.

## Quick Start
Optional (recommended for contributors): create and sync a local environment.
```bash
uv venv --python=3.12
uv sync
```

1. Pick a skill folder in `skills/` and open its `SKILL.md`.
2. Use it directly in your assistant by pasting relevant sections or pointing the assistant to the file path.
3. For development tasks (creating, validating, packaging skills), follow the commands in `AGENTS.md`.

Example: initialize a new skill using the repo tooling.
```bash
uv run skills/skill-creator/scripts/init_skill.py <skill-name> --path skills/
```

## Install as Claude Code Plugin
1. Add the marketplace:
```
/plugin marketplace add soba-labs/langchain-agent-skills
```
2. Install a plugin bundle:
```
/plugin install langgraph-skills@soba-labs-langchain-agent-skills
```
Three bundles are available: `langgraph-skills` (project setup, agent patterns, state management, error handling), `langsmith-skills` (trace analyzer, deployment) and `deepagents-skills` (setup and configuration, planning and todos). Each bundle also ships `skill-creator`.

Or use the interactive menu:
```
/plugin menu
```
For local development:
```
claude --plugin-dir ./path/to/langchain-agent-skills
```
Once installed, Claude Code will automatically use these skills when relevant.

## Use in Other Repositories
These skills can be shared by copying a skill folder (for example `skills/langgraph-agent-patterns/`) into another repository or a supported assistant skills directory.

### OpenAI Codex
Codex reads skills from `.agents/skills` in a repo and `~/.agents/skills` globally
([Codex customization docs](https://learn.chatgpt.com/docs/customization/overview#skills)).
The `SKILL.md` format is identical to Claude Code's, so no conversion is needed.

```bash
git clone https://github.com/soba-labs/langchain-agent-skills.git

# For one project
mkdir -p .agents/skills && cp -r langchain-agent-skills/skills/* .agents/skills/

# Or for every project
mkdir -p ~/.agents/skills && cp -r langchain-agent-skills/skills/* ~/.agents/skills/
```

This repository also ships a Codex plugin manifest at `.codex-plugin/plugin.json`, so it can be
installed as a Codex plugin from a local marketplace rather than copied by hand.

### OpenCode
OpenCode searches the widest set of locations and will find these skills in any of them. Per project
it reads `.opencode/skills/`, `.claude/skills/` and `.agents/skills/`; globally it reads
`~/.config/opencode/skills/`, `~/.claude/skills/` and `~/.agents/skills/`. Either placement above
therefore works unchanged, or:

```bash
mkdir -p .opencode/skills && cp -r langchain-agent-skills/skills/* .opencode/skills/
```

### Cursor
Option 1: Remote rule (GitHub)
- Cursor Settings → Rules → Add Rule → Remote Rule (GitHub)
- Use: `https://github.com/soba-labs/langchain-agent-skills.git`

Option 2: Local installation
```bash
# Project-level
git clone https://github.com/soba-labs/langchain-agent-skills.git .cursor/skills/agent-skills

# User-level
git clone https://github.com/soba-labs/langchain-agent-skills.git ~/.cursor/skills/agent-skills
```
Usage: type `/` in Agent chat to search and select skills by name.

### Other Assistants
If your assistant does not support skills directly, point it at the skill file:
```
Read skills/langsmith-deployment/SKILL.md for production deployment guidance
Read skills/langgraph-agent-patterns/SKILL.md for multi-agent patterns
```

## Harness Compatibility

The skills themselves are harness-agnostic: one `SKILL.md` with `name` and `description` frontmatter,
optional `scripts/`, `references/` and `assets/`. Claude Code, Codex and OpenCode all read that same
format and all use progressive disclosure, so nothing in `skills/` is specific to one assistant. Only
discovery and distribution differ, and this repository ships all three:

- **Claude Code** reads `.claude/skills/` (project) and `~/.claude/skills/` (global); distribution via
  `.claude-plugin/marketplace.json`.
- **Codex** reads `.agents/skills/` (project) and `~/.agents/skills/` (global); distribution via
  `.codex-plugin/plugin.json`.
- **OpenCode** reads `.opencode/skills/`, `.claude/skills/` and `.agents/skills/`, plus the global
  equivalents, so it is satisfied by either of the above.

The repo root carries `.claude/skills` and `.agents/skills` as symlinks to `skills/`, which means all
three assistants discover the skills when this repository is itself the working project (contributors
get them for free). Windows contributors may need `git config core.symlinks true`.

## Repository Structure
- `skills/` - 10 skill packages (9 production skills + skill-creator)
- `.claude-plugin/marketplace.json` - Marketplace manifest for Claude Code
- Optional `*.skill` exports - Generated distribution artifacts
- `PLAN.md` - Roadmap and implementation ordering notes
- `AGENTS.md` - Complete development guidelines and workflow

## Contributing
Community contributions are welcome. We want to build great, comprehensive, and widely used LangChain agent skills together.

1. Open an issue for bugs, feature ideas, new skills, or improvements. Include context, expected behavior, and reproducible steps when possible.
2. Fork the repo and create a focused branch for your change.
3. Follow `AGENTS.md` while implementing:
   - Use `uv run` for Python scripts.
   - Keep skill instructions concise and move deep details to `references/`.
   - Validate skill structure with `uv run skills/skill-creator/scripts/quick_validate.py skills/<skill-name>/`.
4. Open a PR and include:
   - Clear summary of what changed and why.
   - Reference to related issue or `PLAN.md` item (if applicable).
   - Validation notes (script checks, `quick_validate.py` output, manual verification).
5. Request review and respond to feedback quickly so we can merge safely.

Keep changes focused and actionable. `AGENTS.md` is the single source of truth for workflow, tooling, and architecture rules.
