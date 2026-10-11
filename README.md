# Marcapony for Claude

Build and edit your [Marcapony](https://marcapony.com) website from Claude. Describe a section you need, such as a pricing table, a booking banner or an animated hero, and Claude writes it as a custom code section: a React component styled with Tailwind that Marcapony compiles, renders on the server and shows on your page. Claude can also change existing custom code sections, your custom navbar and footer, and reuse components from your account library.

Everything Claude saves on your pages is a draft. Your live site keeps showing the last published version until you publish from the Marcapony dashboard; Claude cannot publish your site. Deleting pages, products and orders, cancelling, and bulk status changes are left to you in the dashboard. Every change Claude makes is recorded in your account's activity log, and every custom code save is kept as a version you can go back to.

## What is in this plugin

- **The Marcapony connector**: a remote MCP server at `https://mcp.marcapony.com/mcp`. It is the only service the plugin talks to.
- **The `custom-code` skill**: the order of work Claude follows for custom code (read your preferences and theme, find the page, read the authoring guide, write, check the compile result and a preview, report what changed).
- **Commands**: `/marcapony:new-section` to create a section and `/marcapony:edit-section` to change one.

The connector also exposes the rest of your account (pages, blog posts, products, navigation, SEO, media) through the same connection, within the permissions of your role. This first release only adds guidance for custom code.

## Requirements

- A Marcapony account. Your role on the account decides what Claude can change; the connector enforces the same permissions as the dashboard.
- Claude with connector and plugin support (claude.ai, the Claude desktop app, or Claude Code).

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

When Claude is done, open the Marcapony dashboard, review the page and publish.

## Data and privacy

The plugin sends your requests and the content Claude reads or writes only to `mcp.marcapony.com`, which acts on your Marcapony account through the Marcapony API. It does not read files on your computer and runs no local code. Marcapony's handling of your data is described in the [Privacy Policy](https://app.marcapony.com/privacy-policy) and the [Terms of Service](https://app.marcapony.com/terms-of-service).

## Support

Email [hello@marcapony.com](mailto:hello@marcapony.com) or use [marcapony.com/iletisim](https://marcapony.com/iletisim).

## License

MIT. See [LICENSE](LICENSE).
