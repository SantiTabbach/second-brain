---
tags: [areas, software, ai, subagents]
author: Santi Tabbach
created: 2026-05-03
modified: 2026-05-03
---

Subagents are specialized assistants that Claude Code can delegate tasks to. Think of them as focused helpers: each one runs in its own conversation context window, does its work, and returns a summary to the main thread. The intermediate steps -- all the file reads, searches, and tool calls -- stay isolated and never clutter your main conversation.

## Why Subagents Matter

Every time you chat with Claude Code, you're adding to the main context window. Every tool call, every file read, every search result gets stored there. That space is finite, and once it fills up, Claude starts losing track of earlier parts of the conversation.

Subagents solve this by spinning up a separate context window. The subagent receives two things:

- **A custom system prompt** from your configuration file that defines the subagent's role and behavior
- **A task description** written by the parent agent based on what you asked for

The subagent then works on its own. It reads files, runs searches, edits code -- whatever it needs to do. When it's done, only a summary comes back to your main conversation. The entire subagent conversation is then discarded.

This means your main context stays clean. You get the answer without all the noise of the journey it took to find it. The tradeoff is that you lose visibility into how the subagent reached its conclusions.

## Built-in Subagents

Claude Code ships with several built-in subagents you can use right away:

- **General purpose subagent** -- for multi-step tasks that require both exploration and action
- **Explore** -- for fast searching and navigation of codebases
- **Plan** -- used during plan mode for research and analysis of your codebase before presenting a plan

## Custom Subagents

Beyond the built-in options, you can create your own subagents with custom system prompts and tool access. This lets you define specialized agents tailored to your workflow -- a code reviewer, a test writer, a documentation generator, or anything else you need.

---

## Key Takeaways

Subagents give you three main benefits:

- They break work into focused pieces, letting each subagent concentrate on a specific task
- They keep your main context window clean by isolating all the intermediate work
- They bring back just the information you need as a concise summary

---

[[Creating a Subagent]]
[[Designing effective Subagents]]
[[Using Subagents effectively]]