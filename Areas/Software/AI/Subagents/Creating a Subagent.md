---
tags: [areas, software, ai, subagents]
author: Santi Tabbach
created: 2026-05-03
modified: 2026-05-03
---

## Creating a Subagent (Claude)

The easiest way to create a subagent is with the `/agents` slash command. This opens the main interface for managing your subagents. From there, select **Create new agent**.

You will first be asked to choose the scope of your subagent:

- **Project-level** -- available only in the current project
- **User-level** -- shared across all projects on your machine

Next, you can choose how to create it. You can write the configuration manually, but the recommended approach is to let Claude generate it for you. Just describe what you want the subagent to do, and Claude will produce a name, description, and system prompt based on your input.

## Customizing Tools

During creation, you get the chance to customize which tools the subagent can access. The tool categories include:

- Read-only tools
- Edit tools
- Execution tools
- MCP tools
- Other tools

## Choosing a Model and Color

After configuring tools, you select which Claude model powers the subagent. Your options are:

- **Haiku** -- best for fast, lightweight tasks
- **Sonnet** -- a good middle ground between speed and depth
- **Opus** -- best for complex analysis
- **Inherit** -- uses whatever model your main conversation is running

Finally, you pick a color. This shows up in the UI so you can quickly tell which subagent is active. It is a small touch, but it helps when you have multiple subagents running.

---

## The Config File

Once creation is complete, the subagent config file is saved into your project (typically at `.claude/agents/your-agent-name.md`). Here is what a typical subagent config looks like:

```
---
name: code-quality-reviewer
description: Use this agent when you need to review recently written or modified code for quality, security, and best practice compliance.
tools: Bash, Glob, Grep, Read, WebFetch, WebSearch
model: sonnet
color: purple
---

You are an expert code reviewer specializing in quality assurance, security best practices, and
adherence to project standards. Your role is to thoroughly examine recently written or modified code
and identify issues that could impact reliability, security, maintainability, or performance.
```

Let's break down each field:

- **`name`** -- A unique identifier for the subagent. This is how you reference it, either by asking Claude directly or by typing `@agent code-quality-reviewer` in your message.
- **`description`** -- Controls when Claude decides to use the subagent. This must be a single line (use escaped newline characters `\n` if you need breaks). You can include example conversations here to help Claude understand when delegation is appropriate.
- **`tools`** -- Lists which tools the subagent can access. This matches whatever you selected during generation, but you can edit the list here at any time.
- **`model`** -- Specifies which Claude model to use: `sonnet`, `opus`, `haiku`, or `inherit`.
- **`color`** -- The UI color for identifying the subagent.

---

## System Prompts

The body of the markdown file (everything below the YAML frontmatter) is the system prompt. This is where you give the subagent its instructions.

---

## Making Claude Use Your Subagent Automatically

If you want Claude to delegate tasks to the subagent without you explicitly asking, include the word **"proactively"** in the description field. For example:

```
description: Proactively suggest running this agent after major code changes...
```

You can also add example conversations to the description to help Claude understand specific scenarios where the subagent should be used.