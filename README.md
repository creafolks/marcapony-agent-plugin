# Marcapony for Claude

Build and edit your [Marcapony](https://marcapony.com) website from Claude. Describe a section you need, such as a pricing table, a booking banner or an animated hero, and Claude writes it as a custom code section: a React component styled with Tailwind that Marcapony compiles, renders on the server and shows on your page. Claude can also change existing custom code sections, your custom navbar and footer, add scroll animations and 3D scenes with the libraries Marcapony provides, and reuse components from your account or Marcapony's section library. In Claude Code it can also build and deploy a Marcapony app: a small web app with its own backend that Marcapony hosts on your site, in your customer portal or in your dashboard.

Everything Claude saves on your pages is a draft. Your live site keeps showing the last published version until you publish from the Marcapony dashboard; Claude cannot publish your site. Deleting pages, products and orders, cancelling, and bulk status changes are left to you in the dashboard. Every change Claude makes is recorded in your account's activity log, and every custom code save is kept as a version you can go back to.

## What is in this plugin

- **The Marcapony connector**: a remote MCP server at `https://mcp.marcapony.com/mcp`. It is the only service the plugin connects to.
- **The `custom-code` skill**: the order of work Claude follows for custom code sections, navbars and footers (read your preferences and theme, find the page, read the authoring guide and the available libraries, reuse a library section where one fits, write, check the compile result and a preview, report what changed). It covers GSAP with ScrollTrigger, three.js and smooth scroll through the hooks Marcapony provides.
- **The `app-development` skill**: the order of work for a Marcapony app in Claude Code (scaffold with `create-marcapony-app`, link with the `marcapony` CLI, build with `marcapony-sdk`, test in test mode, deploy).

In Claude Code the skills can also be started directly: `/marcapony:custom-code` and `/marcapony:app-development`.

The connector also exposes the rest of your account (pages, blog posts, products, navigation, SEO, media) through the same connection, within the permissions of your role.

## Requirements

- A Marcapony account. Your role on the account decides what Claude can change; the connector enforces the same permissions as the dashboard.
- Claude with connector and plugin support (claude.ai, the Claude desktop app, or Claude Code).
- For apps: Claude Code with a project folder, [Bun](https://bun.sh), and an app created under **My Apps** in the Marcapony dashboard.

## Install

**From the Claude directory**: open Customize > Plugins (or Connectors), search for Marcapony, and add it.

**In Claude Code**:

```bash
claude plugin marketplace add creafolks/marcapony-claude-plugin
claude plugin install marcapony@marcapony
```

**Connector only**: add `https://mcp.marcapony.com/mcp` as a custom connector in Claude, or in Claude Code run `claude mcp add --transport http marcapony https://mcp.marcapony.com/mcp`.

## Connect your account

The first time Claude uses a Marcapony tool, it opens a sign-in window. Sign in to the Marcapony dashboard, check that the business shown is the one you want Claude to work on, and select Connect. The connection reaches that business only. You can end it at any time from Account > API keys in the dashboard.

## Try it

- "Add a three-tier pricing section to my Pricing page, using my theme colours."
- "Make the call-to-action in the hero section on my home page more prominent."
- "Create a FAQ section with five questions about our delivery times and make each answer editable in the dashboard."
- "Turn the testimonials section on the About page into a carousel."
- "Add a section to the home page where the product images slide in as the visitor scrolls."
- "Create a booking app for my customer portal where customers can see and cancel their appointments." (Claude Code)

When Claude is done with a page, open the Marcapony dashboard, review the page and publish. An app is published by its deploy, starts in test mode, and goes live with real data when you turn test mode off in the dashboard.

## Data and privacy

The plugin contains only instructions for Claude; it ships no scripts and no executables. It connects only to `mcp.marcapony.com`, which acts on your Marcapony account through the Marcapony API, and sends it your requests and the content Claude reads or writes there. The `custom-code` skill works entirely through that connector.

The `app-development` skill is used in Claude Code, in a project folder you choose. There Claude reads and edits the app's files and runs the commands of the Marcapony developer kit: `npx create-marcapony-app`, `bun install` (which downloads the app's dependencies from the npm registry), `bun run dev`, `bun run typecheck` and `bun run deploy`. The deploy uploads the built app to Marcapony. The skill asks you to run the linking step yourself, and Claude never reads, prints or commits the app key that linking writes to `.dev.vars`. Claude Code's permission settings decide which of these commands need your approval. Marcapony's handling of your data is described in the [Privacy Policy](https://app.marcapony.com/privacy-policy) and the [Terms of Service](https://app.marcapony.com/terms-of-service).

## Support

Email [hello@marcapony.com](mailto:hello@marcapony.com) or use [marcapony.com/iletisim](https://marcapony.com/iletisim).

## License

MIT. See [LICENSE](LICENSE).
