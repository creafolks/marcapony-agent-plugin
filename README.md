# Marcapony for AI agents

Build and edit your [Marcapony](https://marcapony.com) website with your AI assistant. Describe a section you need, such as a pricing table, a booking banner or an animated hero, and the assistant writes it as a custom code section: a React component styled with Tailwind that Marcapony compiles, renders on the server and shows on your page. It can also change existing custom code sections, your custom navbar and footer, add scroll animations and 3D scenes with the libraries Marcapony provides, and reuse components from your account or Marcapony's section library. In a coding agent with a project folder it can also build and deploy a Marcapony app: a small web app with its own backend that Marcapony hosts on your site, in your customer portal or in your dashboard.

The plugin is a Marcapony connector (a remote MCP server) and two skills in the open [Agent Skills](https://agentskills.io) format. It installs as a plugin in Claude, and the connector and skills also work in other agents that support MCP and Agent Skills.

Sections the assistant saves on your pages are drafts. Your live site keeps showing the last published version until you publish from the Marcapony dashboard; the assistant cannot publish your whole site. A custom navbar or footer is different: it goes live on every page as soon as it is saved, so the assistant asks you before it writes one. It can also put a single page live, publish a blog post, or schedule a page or post, but only when you ask it to. Deleting pages, products and orders, cancelling, and bulk status changes are left to you in the dashboard. Every change made through the connector is recorded in your account's activity log, and every custom code save is kept as a version you can go back to.

## What is in this plugin

- **The Marcapony connector**: a remote MCP server at `https://mcp.marcapony.com/mcp`. It is the only service the plugin connects to.
- **The `custom-code` skill** (`skills/custom-code`): the order of work for custom code sections, navbars and footers (read your preferences and theme, find the page, read the authoring guide and the available libraries, reuse a library section where one fits, write, check the compile result and a preview, report what changed). It covers GSAP with ScrollTrigger, three.js and smooth scroll through the hooks Marcapony provides. The authoring guide it reads comes from the Marcapony connector itself, so it always matches the platform version your site runs on.
- **The `app-development` skill** (`skills/app-development`): the order of work for a Marcapony app in a project folder (scaffold with `create-marcapony-app`, link with the `marcapony` CLI, build with `marcapony-sdk`, test in test mode, deploy).

The connector also exposes the rest of your account (pages, blog posts, products, navigation, SEO, media) through the same connection, within the permissions of your role.

## Requirements

- A Marcapony account. Your role on the account decides what the assistant can change; the connector enforces the same permissions as the dashboard.
- An assistant that supports remote MCP servers with sign-in, such as Claude (claude.ai, the desktop app or Claude Code).
- For apps: a coding agent with a project folder and a terminal, [Bun](https://bun.sh), and an app created under **My Apps** in the Marcapony dashboard.

## Install

### Claude

**From the Claude directory**: open Customize > Plugins (or Connectors), search for Marcapony, and add it.

**In Claude Code**:

```bash
claude plugin marketplace add creafolks/marcapony-agent-plugin
claude plugin install marcapony@marcapony
```

In Claude Code the skills can also be started directly: `/marcapony:custom-code` and `/marcapony:app-development`.

**Connector only**: add `https://mcp.marcapony.com/mcp` as a custom connector in Claude, or in Claude Code run `claude mcp add --transport http marcapony https://mcp.marcapony.com/mcp`.

### Other agents

1. Add `https://mcp.marcapony.com/mcp` as a remote (streamable HTTP) MCP server in your agent's MCP settings.
2. Copy the folders in `skills/` to the folder your agent reads skills from. Most agents read `.agents/skills/` in the project or `~/.agents/skills/` in your home folder; some also have their own folder, such as `.cursor/skills/`, `.github/skills/` or `.gemini/skills/`. See your agent's documentation.

## Connect your account

The first time the assistant uses a Marcapony tool, it opens a sign-in window. Sign in to the Marcapony dashboard, check that the business shown is the one you want the assistant to work on, and select Connect. The connection reaches that business only. You can end it at any time from Account > API keys in the dashboard.

## Try it

- "Add a three-tier pricing section to my Pricing page, using my theme colours."
- "Make the call-to-action in the hero section on my home page more prominent."
- "Create a FAQ section with five questions about our delivery times and make each answer editable in the dashboard."
- "Turn the testimonials section on the About page into a carousel."
- "Add a section to the home page where the product images slide in as the visitor scrolls."
- "Create a booking app for my customer portal where customers can see and cancel their appointments." (in a coding agent with a project folder)

When the assistant is done with a page, open the Marcapony dashboard, review the page and publish. An app is published by its deploy, starts in test mode, and goes live with real data when you turn test mode off in the dashboard.

## Data and privacy

The skills contain only instructions for the assistant, and the plugin runs no scripts or executables of its own when you use it. The `evals/` folder is the plugin's test suite; it is not loaded when you use the plugin. It connects only to `mcp.marcapony.com`, which acts on your Marcapony account through the Marcapony API, and sends it your requests and the content the assistant reads or writes there. The `custom-code` skill works entirely through that connector.

The `app-development` skill is used in a coding agent, in a project folder you choose. There the assistant reads and edits the app's files and runs the commands of the Marcapony developer kit: `npx create-marcapony-app`, `bun install` (which downloads the app's dependencies from the npm registry), `bun run dev`, `bun run typecheck` and `bun run deploy`. The deploy uploads the built app to Marcapony. The skill asks you to run the linking step yourself, and tells the assistant never to read, print or commit the app key that linking writes to `.dev.vars`. Your agent's permission settings decide which of these commands need your approval. Marcapony's handling of your data is described in the [Privacy Policy](https://app.marcapony.com/privacy-policy) and the [Terms of Service](https://app.marcapony.com/terms-of-service).

## Tests

`evals/` holds the plugin's eval suite for [`claude plugin eval`](https://code.claude.com/docs/en/plugin-evals). Each case gives Claude a request and checks what the skills make it do: which Marcapony tools it calls and in what order, what it writes, and what it tells the user. The Marcapony connector is answered by the mocks in `evals/mocks/marcapony`, so a run never reaches a real account. The suite runs on every pull request that changes the skills, the manifest or the suite.

To run it yourself:

```sh
claude plugin eval . --scaffold --allow-tools Write Edit
```

`--scaffold` builds the sample app project the app cases work in, and `Write` and `Edit` let Claude change that project inside the run's own workspace.

## Support

Email [hello@marcapony.com](mailto:hello@marcapony.com) or use [marcapony.com/iletisim](https://marcapony.com/iletisim).

## License

MIT. See [LICENSE](LICENSE).
