---
tags: [areas, software, ai, claude]
author: Santi Tabbach
created: 2026-04-25
modified: 2026-04-25
---

### Finding and connecting tools

Anthropic maintains a directory of recommended connectors at claude.ai/directory. The directory is organized into two tabs:

- **Web:** Cloud services and applications (Gmail, Notion, Slack, Asana, Linear, Stripe, and many more)
- **Desktop extensions:** Local tools that run on your computer through the Claude Desktop app

#### Setting up a web connector

Here's how to connect a cloud service:

1. **Find the connector:** Navigate to claude.ai/directory or click "Search and tools" > "Add connectors" in any chat
2. **Click Connect:** Select the connector you want to add
3. **Authenticate:** You'll be redirected to the service's login page. Sign in with your existing credentials
4. **Grant permissions:** Review the specific permissions Claude is requesting, then authorize access
5. **Test the connection:** Return to Claude and try a simple request, like "Can you access my [tool name]?"
#### Desktop extensions

Desktop extensions require the Claude Desktop app rather than the web interface. These extensions let Claude interact with local applications, your file system, and native features on macOS or Windows.

Some desktop extensions include:

- Local file access for reading and organizing documents
- Browser control for automated web tasks
- Native application integration (like Figma for design work)

To install a desktop extension:

1. Download and install the [Claude Desktop app](https://claude.ai/download)
2. Open the app and navigate to Settings > Extensions
3. Browse available extensions and click Install
4. Follow any additional setup steps specific to that extension

### Security and permissions

When you connect Claude to external services, you're granting it access to read—and sometimes modify—data within those services. Here are some important considerations:

- **Scoped access:** Permissions are specific to what the connector needs and you can toggle individual permissions on and off within each application's menu.
- **Claude sees what you see:** Claude can only access data _you_ have access to. Connecting your work email doesn't give Claude access to your CEO's inbox—only your own.
- **Revocable at any time:** You can disconnect a service through Claude's settings or through the third-party service's security settings. Just as with Skills, you can also find or build custom connectors. Exercise the same caution — only install connectors from trusted sources.