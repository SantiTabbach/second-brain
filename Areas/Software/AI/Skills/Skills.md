---
tags: [areas, software, ai, skills]
author: Santi Tabbach
created: 2026-04-25
modified: 2026-05-05
---

## What Skills Are

Skills are folders of instructions and resources that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a name and description in its frontmatter.

The description is how Claude decides whether to use the skill. When you ask Claude to review a PR, it matches your request against available skill descriptions and finds the relevant one. Claude reads your request, compares it to all available skill descriptions, and activates the ones that match.

```markdown
--- 
name: pr-review 
description: Reviews pull requests for code quality. Use when reviewing PRs or checking code changes. 
---
```

Below the frontmatter, you write the actual instructions — your review checklist, formatting preferences, or whatever Claude needs to know for that task.

## Where Skills Live

You can store skills in different places depending on who needs them:

- **Personal skills** go in `~/.claude/skills` (your home directory). These follow you across all your projects — your commit message style, your documentation format, how you like code explained.
- **Project skills** go in `.claude/skills` inside the root directory of your repository. Anyone who clones the repo gets these skills automatically. This is where team standards live, like your company's brand guidelines, preferred fonts, and colors for web design.

On Windows, personal skills live in `C:/Users/<your-user>/.claude/skills`.

Project skills get committed to version control alongside your code, so the whole team shares them.

## When to Use Skills

Skills work best for specialized knowledge that applies to specific tasks:

- Code review standards your team follows
- Commit message formats you prefer
- Brand guidelines for your organization
- Documentation templates for specific types of docs
- Debugging checklists for particular frameworks

The rule of thumb is simple: if you find yourself explaining the same thing to Claude repeatedly, that's a skill waiting to be written.

---
## Skills vs. CLAUDE.md vs. Slash Commands

Claude Code has several ways to customize behavior. Skills are unique because they're automatic and task-specific. Here's how they compare:

- **CLAUDE.md** files load into every conversation. If you want Claude to always use TypeScript's strict mode, that goes in CLAUDE.md.
- **Skills** load on demand when they match your request. Claude only loads the name and description initially, so they don't fill up your entire context window. Your PR review checklist doesn't need to be in context when you're debugging — it loads when you actually ask for a review.
- **Slash commands** require you to explicitly type them. Skills don't. Claude applies them when it recognizes the situation.

When Claude matches a skill to your request, you'll see it load in the terminal.

---
#### Types of Skills

There are two categories of Skills you'll encounter:

- **Anthropic Skills** are created and maintained by Anthropic. These include enhanced document creation capabilities for Excel, Word, PowerPoint, and PDF files. Anthropic Skills are available to all paid users and Claude invokes them automatically when relevant—you don't need to do anything special to use them.
- **Custom Skills** are ones you or your organization create for specialized workflows and domain-specific tasks. For example, you might create a skill that applies your company's brand guidelines to presentations, structures meeting notes in a specific format, or executes your organization's data analysis workflows.

#### Security considerations

Because Skills can include executable code, it's important to use them thoughtfully:

- Only install custom Skills from trusted sources
- Anthropic's built-in Skills are tested and maintained by Anthropic
- Custom Skills you upload are private to your individual account
- If you're installing a custom Skill from an external source, review its contents before use to understand what it does.

#### Key takeaways

- **Skills are folders of instructions** that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a name and description in its frontmatter
- **Claude uses the description to match skills to requests.** When you ask Claude to do something, it compares your request against available skill descriptions and activates the ones that match
- **Personal skills** go in `~/.claude/skills` and follow you across all projects. **Project skills** go in `.claude/skills` inside a repository and are shared with anyone who clones it
- **Skills load on demand** — unlike CLAUDE.md (which loads into every conversation) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
- If you find yourself **explaining the same thing to Claude repeatedly**, that's a skill waiting to be written

---

[[Creating custom Skills]]
[[Configuration and multi-file skills]]
[[Skills vs Other Claude Code Features]]
[[Sharing Skills]]
[[Troubleshooting skills]]