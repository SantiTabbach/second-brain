---
tags: [areas, software, ai, claude, claude-code]
author: Santi Tabbach
created: 2026-05-06
modified: 2026-05-06
---

## The Agentic Loop

Claude Code is best explained through the **agentic loop**:

1. You enter a prompt into Claude Code.
2. Claude gathers the context it needs by interacting with the model, which returns text or a tool call that Claude Code can execute.
3. It takes action — for example, editing a file or running a command.
4. It verifies the results and determines whether they achieve what your prompt set out to do.
5. If they do, Claude finishes and waits for the next prompt. If they don't, it loops back and tries again until the results are complete and verifiable.

Throughout this loop, you can add context, interrupt, or steer the model to help guide it toward your goal.

![[Pasted image 20260506202152.png]]
## Context

Claude has a **context window** that determines how much of your conversation, file contents, command outputs, and more it can store and reference. Once you reach that limit, Claude Code compacts your conversation — automatically determining what it can remove or summarize to bring the context window back down to a usable size.

## Tools

Tools are the backbone of how agents work. Most AI assistants simply take text in and return text out. Tools let Claude Code determine _when_ to execute code to get closer to completing a task. This could be a file-reading tool, a web search tool, or any number of other capabilities. Claude Code uses semantic understanding to determine when to call a tool and how to use the output.

## Permissions

Claude Code has several permission modes:

- **Default behavior:** Claude asks for explicit permission before editing a file or running a shell command.
- **Auto-accept:** Files are edited without asking, but commands still require approval.
- **Plan mode:** Uses read-only tools to compile a plan of action before starting any work.

---
## Auto-Accept vs. Approval

You can choose whether Claude auto-accepts every file change it suggests, or whether it asks for your explicit permission each time. Press `Shift + Tab` to cycle between modes.

- **Approval mode:** Claude asks permission each time it wants to edit a file or run a command.
- **Auto-accept mode:** File edits are automatically approved, but commands still require your permission.

There's no right or wrong answer — it's whatever you're comfortable with.

## Plan Mode

Within the `Shift + Tab` menu is **Plan Mode**. Plan mode takes your prompt and uses read-only tools to analyze your codebase and research your suggested implementation. It will ask clarifying questions along the way, then return a detailed plan it can execute.

Plan mode is great for planning complex changes or doing a safe code review. Many times you'll be asking Claude to handle multi-step implementations toward a feature, and this is exactly where Plan Mode excels.