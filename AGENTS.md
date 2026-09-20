# AGENTS.md

This is the model-neutral entrypoint for AI agents working in this node.

## Start Here

Read these in order:

1. `AGENTS.md` (this file — project identity and rules)
2. `.agents/skills/project-orientation/SKILL.md` (**★ READ THIS FIRST — full architecture, data flow, tech debt**)
3. `.agents/README.md` (agent layer structure)
4. The nearest workflow, tracker, or playbook file for the task

> [!IMPORTANT]
> **Do NOT scan the entire codebase.** The project-orientation skill has everything you need to get started. If it doesn't answer your question, update it after you find the answer.

## Project Mission

`m3e-canvas` is an interactive canvas for sketching Material 3 Expressive (M3E) screens directly in the browser and turning them into vibe-coding prompts. Built by `lnkiai`, it integrates Next.js, React 19, Motion, and Tailwind CSS v4.

## Node Map

| Path | Purpose |
|---|---|
| `app/` | Next.js App Router root layout, page routing, and canvas workspace |
| `components/` | Reusable UI components, Material 3 Expressive controls, and canvas widgets |
| `lib/` | Core logic, export engines, color systems, and prompt generators |
| `docs/` | Project documentation and architecture specs |
| `public/` | Static web assets and fonts |
| `.agents/` | Agent operating system — architecture rules, skills, commands, hooks, subagents |
| `Current_Status/` | Local milestone and session snapshot logs |

## Non-Negotiables

### Agent Identity & Co-Ma Architecture
You are an instance of **Co-Ma** (Context-Matrix). Your foundational persona and prime directives are defined in [CO_MA_PERSONA.md](file:///.agents/CO_MA_PERSONA.md). 
You must read that file to understand your overarching mission. You must also read [LOCAL_CONTEXT.md](file:///.agents/LOCAL_CONTEXT.md) to understand the current focus and active blockers in this specific node.

- **Use internal contracts for non-trivial work** - define what must be true at completion before treating a task as done.
- **Read project-orientation skill first** — don't waste tokens scanning files.
- Preserve project structure and abide by all rules in `.agents/architecture/`.
- Read local instructions before editing.
- Do not edit unrelated files.
- Do not invent evidence, data, APIs, or requirements.
- Stop when a human gate is explicitly marked.
- Report changed files and remaining gaps.
- **Update project-orientation skill** after any architectural change.

## Common Workflows

- Use `.agents/skills/project-orientation/` to understand the project.
- Use `.agents/skills/internal-contracts/` to define success criteria and verification for meaningful tasks.
- Use `.agents/architecture/` for mandatory code structure, sizing limits, and SOLID rules.
- Use `.agents/commands/` for explicit repeatable tasks.
- Use `.agents/skills/` for reusable workflow knowledge.
- Use `.agents/subagents/` for specialist roles.
- Use `.agents/hooks/` for deterministic guardrails.
