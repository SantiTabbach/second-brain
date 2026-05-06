---
tags: [areas, software, ai, claude, claude-code, daily-workflows]
author: Santi Tabbach
created: 2026-05-06
modified: 2026-05-06
---

## Explore and Plan

The fastest way to handle these first two steps is with **Plan Mode**. In plan mode, Claude can't edit files — it just reads files to gather information about how it will tackle the implementation.

To enter plan mode, press `Shift + Tab` until you see "Plan Mode" under the text input. Then write a prompt like:

```
I need to add WebP conversion to our image upload pipeline. Figure out where in the pipeline it should happen, whether we need new dependencies, and how to approach it.
```

Claude will read relevant files, run some web searches, and give you a plan of action. Review it and decide if it meets your criteria. If not, ask it to revise specific areas.

This is the best place to course-correct because it's before any code is written. You can also run the explore subagent without being in plan mode if you just want a general summary of your codebase without intending to make changes afterward.

---
## Code

Once the plan looks good, select "approve" to accept it and let Claude work through the list items. You can choose whether Claude auto-accepts file edits or asks you each time.

Claude will do its best to troubleshoot before considering the plan "finished," but at times you'll need to step in. This is the benefit of working with Plan Mode — after execution, you also have the context of how you got to the results, which helps guide Claude's next decisions.

A few tips to make the coding phase smoother:

- **Define a success criteria.** For Claude to be confident in its results, it needs to be clear on what "correct" looks like. Make this explicit when writing your plan.
- **Add tools.** Tools that help Claude complete its goals remove a lot of back and forth. For example, if you're building web UIs, install the Claude in Chrome extension so Claude Code can control a browser tab and test the UI directly.
- **Include a test suite.** Give Claude a test suite it can continuously validate against. Claude can even write tests for you. Before handing this off, make sure the tests are a reliable source of truth to avoid false positives.

**Quick tip:** If you find Claude keeps running into the same issues, ask it to save the solution to its CLAUDE.md file.

---
## Commit

Once you've tested the changes yourself and are happy with the results, it's time to push your code. Before you commit, run a **subagent code reviewer** to look at your work. A subagent gets a fresh pair of eyes on the codebase — it doesn't carry the bias the main agent might have from the session.

Then get Claude to generate a commit message in your style. Rinse and repeat.