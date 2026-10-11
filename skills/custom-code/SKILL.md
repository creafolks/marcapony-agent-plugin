---
name: custom-code
description: Build or change a custom code section (a React + Tailwind component) on a Marcapony website, or a custom navbar or footer, through the Marcapony connector. Use when the user wants a new section their site builder does not offer, wants to restyle or rework an existing custom code section, or wants to reuse a component from their account library.
---

# Custom code sections on Marcapony

A custom code section is a React component (JSX plus optional CSS, Tailwind available) that Marcapony compiles on its servers, renders on the server and hydrates on the live page. You write source; you never build, bundle or deploy anything yourself. Every save is a draft and is kept as a version. The site owner publishes from the Marcapony dashboard; you cannot publish, so always tell the user their changes are saved and that they publish them from the dashboard.

The tool descriptions and the authoring guide are the reference. This skill is the order of work. Do not rely on memory for input types, actions or bindings: read the guide.

## 1. Learn the account

- `list_memories` (or `get_business_info`, which includes them): the user's standing preferences for tone, language and formatting. Apply them.
- `get_theme`: colours, fonts and radii. Use the theme tokens rather than hard-coded colours so the section follows the site's design.

## 2. Find the target

- The user named a page or a piece of content: `find_content` first. Fall back to `list_pages` only when you need the whole inventory.
- `get_page_details` and `list_sections` show what is already on the page and in which order.
- An existing custom code section: `get_section_details` returns its source, its inputs and its `baseHash`. You need the `baseHash` to edit.
- A navbar or footer: `get_layout_custom_code` instead.

## 3. Read the guide once per session

Call `get_custom_code_guide` before writing (kind `section`, or kind `layout` for a navbar or footer). It covers the rendering model, the wrapper spacing contract, every input type, actions and form submission, product, post and collection bindings, and worked examples. Clients that read MCP resources can use `marcapony://custom-code/authoring-guide` instead.

## 4. Write

- New section: `create_custom_code_section`. Expose the text, images and links the owner will want to change as inputs, so they can edit them in the dashboard without touching code.
- Targeted change to an existing section: `edit_custom_code` with anchored find/replace hunks and the `baseHash` you read. If it is refused with a conflict, read the section again and re-anchor. The background compile also changes `baseHash`, so a second edit right after the first may need a fresh read.
- Rewrite of more than about a third of a component, or a post-bound input: `update_section` with the complete new source.
- Navbar or footer: `write_layout_custom_code`.

Media for inputs: `search_images` searches the account's own images by description (pass `scope: "library"` for the shared public library), `list_recent_uploads` shows what the user just uploaded, and `upload_asset` adds a file from a public URL to the media library.

## 5. Check your work

- Read the compile result and any warnings that come back with the save, and fix blocking findings before you move on. `empty_ssr_html` means the section renders nothing on the server, which hides it from search engines and the first paint.
- `render_section_preview` returns a PNG of the section as the site renders it, with the theme and wrapper spacing applied. It is a server-render snapshot: effects, animations and hover states do not appear, so do not treat a hidden entry animation as a bug.

## 6. Reuse

Before building something common (pricing table, testimonial carousel, FAQ), check `list_library_components` and `get_library_component`. `install_library_component` places an approved component on a page as a linked section: editors fill in its content, and it keeps receiving library updates. When the user wants to reuse a component of their own across pages, `submit_library_component` proposes it for the account library, where a reviewer approves it, and `update_library_component` revises a submission.

## 7. Finish

Summarise what changed and where (page, section), and say plainly that the changes are drafts the owner publishes from the Marcapony dashboard.
