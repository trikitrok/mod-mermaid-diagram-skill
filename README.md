# Mermaid Diagram Skill

A coding agent skill that generates beautiful and practical Mermaid diagrams from natural language descriptions. Not just boxes-and-arrows — diagrams that **argue visually**.

Compatible with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [OpenCode](https://github.com/nicepkg/OpenCode), [GitHub Copilot](https://docs.github.com/en/copilot), and any agent that supports the [Agent Skills](https://agentskills.io) standard.

## What Makes This Different

- **Diagrams that argue, not display.** Every shape and group mirrors the concept it represents — fan-outs for one-to-many, timelines for sequences, convergence for aggregation. No uniform card grids.
- **Evidence artifacts.** Technical diagrams use supplied or verified code snippets, JSON payloads, and precise protocol event names when they improve understanding.
- **Built-in visual validation.** The render pipeline lets the agent see its own output, catch syntax errors, and fix layout issues in a loop before delivering.
- **Brand-customizable.** All styles live in one file (`skills/mermaid-diagram/references/mermaid-theme.md`). Diagrams use `classDef` semantic styles with explicit fill, stroke, and text colors — making them self-contained and readable in both light and dark mode on GitHub, VS Code, Obsidian, or any Mermaid-capable viewer.

## Changes in This Fork

Compared with the [upstream Mermaid Diagram Skill](https://github.com/mgranberry/mermaid-diagram-skill), this fork adds:

- **Source-of-truth guardrails.** When another skill supplies an analyzed model, the diagram skill treats it as authoritative, preserves uncertainty, and does not repeat repository discovery or analysis.
- **No fabricated technical details.** APIs, relationships, states, payloads, and evidence artifacts must come from the supplied model or verified source material. Direct requests gather only the information needed to construct the diagram.
- **Portable rendering.** The render workflow resolves the skill directory from `SKILL.md` instead of assuming a Claude-specific installation path.
- **Stronger validation criteria.** The quality checklist separately checks source integrity, diagram design, Mermaid syntax, visual layout, and cleanup.
- **Corrected repository layout.** The installable skill is stored in `skills/mermaid-diagram/`, with its `SKILL.md` and supporting references inside that directory.

## Installation

This skill follows the [Agent Skills](https://agentskills.io) open standard — a `SKILL.md` file in a folder with supporting resources. Install it wherever your agent looks for skills.

### Claude Code / OpenCode

These agents read from `.claude/skills/`:

```bash
git clone https://github.com/trikitrok/mod-mermaid-diagram-skill.git
cp -r mod-mermaid-diagram-skill/skills/mermaid-diagram .claude/skills/mermaid-diagram
```

### GitHub Copilot (VS Code, CLI, and coding agent)

Copilot supports Agent Skills natively ([docs](https://code.visualstudio.com/docs/copilot/customization/agent-skills)). It reads from `.github/skills/`, `.claude/skills/`, and `.agents/skills/`:

```bash
git clone https://github.com/trikitrok/mod-mermaid-diagram-skill.git
cp -r mod-mermaid-diagram-skill/skills/mermaid-diagram .github/skills/mermaid-diagram
```

Once installed, Copilot discovers the skill automatically from the `SKILL.md` frontmatter. You can invoke it via the `/mermaid-diagram` slash command in chat, or just ask Copilot to create a diagram and it will load the skill when relevant.

> **Tip:** Type `/skills` in the Copilot chat input to see all available skills and verify it was detected.

### Other agents

Any agent that supports the [Agent Skills](https://agentskills.io) standard can use this skill. Copy the folder to your agent's skills directory and ensure the `SKILL.md` file is at the root of the skill folder.

For this repository, the skill folder is `skills/mermaid-diagram`.

## Setup

Requires Node.js and npm (for npx). No other installs needed — the render pipeline uses `npx --yes @mermaid-js/mermaid-cli` which downloads mermaid-cli ephemerally on first use.

## Usage

Ask your coding agent to create a diagram:

> "Create a Mermaid sequence diagram showing how the AG-UI protocol streams events from an AI agent to a frontend UI"

The skill handles the rest — concept mapping, diagram type selection, layout optimization, rendering, and visual validation.

## Customize

Edit `skills/mermaid-diagram/references/mermaid-theme.md` — the single source of truth for all theming. It contains:
- Dark/light mode color guidelines (ensures diagrams are readable in both themes)
- `classDef` semantic style recipes (trigger, success, error, ai, decision, primary)
- Per-diagram-type support notes

## File Structure

| File | Description |
|------|-------------|
| `skills/mermaid-diagram/SKILL.md` | Design methodology + workflow |
| `README.md` | This file |
| `.gitignore` | Ignores `*.png`, `*.svg`, `node_modules/` |
| `skills/mermaid-diagram/references/render_mermaid.sh` | Renders `.mmd` to PNG via `npx mmdc` |
| `skills/mermaid-diagram/references/mermaid-theme.md` | Brand theming — global colors, semantic `classDef` recipes, diagram-type support (single customization point) |
| `skills/mermaid-diagram/references/syntax-pitfalls.md` | Common syntax errors and `<br>` compatibility matrix |
| `skills/mermaid-diagram/references/types/flowchart.md` | Flowchart syntax reference |
| `skills/mermaid-diagram/references/types/sequence.md` | Sequence diagram syntax reference |
| `skills/mermaid-diagram/references/types/state.md` | State diagram syntax reference |
| `skills/mermaid-diagram/references/types/class.md` | Class diagram syntax reference |
| `skills/mermaid-diagram/references/types/c4.md` | C4 diagram syntax reference |
| `skills/mermaid-diagram/references/types/mindmap.md` | Mindmap syntax reference |
| `skills/mermaid-diagram/references/types/timeline.md` | Timeline diagram syntax reference |
| `skills/mermaid-diagram/references/types/er-diagram.md` | ER diagram syntax reference |
| `skills/mermaid-diagram/references/types/sankey.md` | Sankey diagram syntax reference |
