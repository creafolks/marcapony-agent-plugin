---
description: Change an existing custom code section on a Marcapony page
argument-hint: "[page or section] [the change]"
---

Change an existing custom code section on the user's Marcapony website using the custom-code skill.

Request: $ARGUMENTS

Find the section (use find_content for a page the user names, then list_sections), read it with get_section_details, and make the smallest change that does what was asked: edit_custom_code with anchored hunks for a targeted change, update_section only for a large rewrite. Check the compile result and a preview, and finish by telling the user the change is saved as a draft that they publish from the Marcapony dashboard.
