---
tags: [areas, software, ai, claude, claude-code]
author: Santi Tabbach
created: 2026-05-06
modified: 2026-05-06
---

### The CLAUDE.md File

One of the most useful features in Claude Code is the CLAUDE.md file. It gives Claude Code persistent memory about your project.
[[CLAUDE md]]

---
### Subagents

Claude can delegate tasks to subagents that break them down and run component tasks in parallel, improving your context management. Each subagent operates in its own isolated context window.
[[Subagents]]

---
### Skills

Skills are folders of instructions and resources that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a name and description in its frontmatter.
[[Skills]]

---
### MCP

Model Context Protocol (MCP) is an open standard that lets Claude Code connect to external tools and data sources. When you ask a question, Claude automatically understands when it should use those tools to better handle your query.
[[MCP]]

---

### HOOKS

Hooks let you run commands at specific points in Claude Code's lifecycle. The key difference between hooks and everything else covered in this course is that hooks are **deterministic** — they always run.