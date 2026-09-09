# opencode.school

Let's build opencode.school, a website that helps people learn how to use OpenCode.

We'll publish it to GitHub as an open-source project under zeke/opencode.school

It will be a static website, hosted on Cloudflare workers with Hono

It should borrow the aesthetic of the official opencode.ai website. It should be clean and simple, with the same typography and color scheme. It should support dark and light mode based on the user's system preferences. 

It should support some kind of flexible lesson format that can grow over time.

The target audience is people who are new to agentic coding and may not be comfortable using a terminal. The lessons should focus on the OpenCode Desktop app as the primary interface. The terminal app exists and should be mentioned, but all instructions and walkthroughs should center on the Desktop experience.

Here are some lessons that should be covered:

- **Getting Started**: Download and install the OpenCode Desktop app. Mention the terminal app as an alternative for advanced users.
- **Configuring OpenCode**: using global config files like ~/.config/opencode/AGENTS.md and ~/.config/opencode/opencode.json
- **AGENTS.md**: reusable instructions for all sessions
- **Permissions**: control what OpenCode can and cannot do
- **Models**: Start with free models, upgrade to OpenCode Go for $10/month, or connect to your existing Anthropic, Claude, or Gemini accounts.
- **Modes**: Start in Plan mode, iterate on the vision, then switch to Build mode when you're ready to execute.
- **MCP Servers**: give your agent access to tools
- **Skills**: teach your agent skills using `npx skills` and the https://skills.sh directory of skills that conform to the https://agentskills.io/ specification.
- **It's Not Just for Coding**: show that non-technical users can use OpenCode for everyday tasks.
- **Workspaces**: Run multiple simultaneous OpenCode sessions on the same codebase without them stepping on your own toes.

Let's use https://nodeschool.io/ in part for inspiration. That was a successful community-supported volunteer-driven educational website about how to learn and use Node.js. We want to do the same here for OpenCode.