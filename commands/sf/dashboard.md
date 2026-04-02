---
name: sf:dashboard
description: Launch the SF Command Center dashboard in your browser
allowed-tools:
  - Bash
  - Read
---
<objective>
Start the SF Command Center local dashboard server and open it in the browser.
</objective>

<execution_context>
@$HOME/.claude/skills/sf-consulting-framework/SKILL.md
</execution_context>

<process>
1. Check if `dashboard/` directory exists in the project root. If not, report that this project was not scaffolded with the SF Command Center and suggest running `/sf:init` to create a new project with dashboard support.
2. Check if `dashboard/node_modules/` exists. If not, run `cd dashboard && npm install` to install dependencies.
3. Start the server: run `node dashboard/server.js` in the background.
4. Report the URL to the user and remind them to open it in their browser.
5. Note: The server will run until the user stops it (Ctrl+C) or closes the terminal.
</process>
