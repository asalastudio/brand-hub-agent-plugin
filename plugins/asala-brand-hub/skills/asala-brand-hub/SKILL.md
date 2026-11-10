---
name: asala-brand-hub
description: Use when working on a client's website, app, emails, ads or other code in a project connected to the Asala Brand Hub (an MCP server named asala_<client>). Pulls the client's brand kit, keeps BRAND.md current, uses approved hub files, and files new work back into the right hub board.
---

# Working with the Asala Brand Hub

The Asala Brand Hub holds each client's brand: logos, colors, fonts, voice, do's and don'ts, approved photography and the
client profile. In a client project it is connected as an MCP server named `asala_<client>` (one per client; the key only
reaches that client). Code lives in this repository; brand lives in the hub.

## Before you change anything

1. Call `get_workspace`, `get_client_profile`, `get_brand_kit` and `get_brand_tokens`.
2. If the hub tools are missing, stop and tell the user to open the client in the hub and use **Open in** to connect
   this project. Do not guess brand values.
3. Create or refresh `BRAND.md` in the project root from what the hub returned (see below). Commit it with your change
   when the user commits.

## BRAND.md

Keep it short and factual. Sections, in this order:

- **Voice**: tone, words to use, words to avoid, legal lines that must appear.
- **Colors**: name, hex, and where each is used. Use these exact values in code (CSS variables or theme tokens).
- **Typography**: families, weights, sizes for headings and body; note licensing limits.
- **Logos**: which version goes on light, dark and photo backgrounds; minimum size; misuse rules.
- **Do's and don'ts**: the hard rules from the brand kit.
- **Source**: "Generated from the Asala Brand Hub on <date>. Edit the brand in the hub, not here."

## While working

- Follow the brand kit strictly. If a request conflicts with it, say so and ask before deviating.
- Need an image, logo or document? `search_assets` first and use approved hub files (`get_asset` gives a temporary
  download URL). Do not invent logos or recreate brand assets. Search with plain words in any order
  (`"spring hero instagram"`): each word may match the name, description, tags or board. Results marked
  `partial_match` matched only some of the words. Pass `references: "exclude"` when you want something to post or
  deliver.
- New assets you produce (images, copy, documents) go back into the hub: `save_document` for writing,
  `start_upload` + upload + `finish_upload` for files. Put them in the board the user named, or the closest matching
  board (`board_path` like `Campaigns/Spring 2026`; missing boards are created).
- Set Status "In review" only when the user asks for review. You can never mark anything Approved: approval is a
  person's sign-off in the hub.
- Never write the hub API key into the repository, BRAND.md, commit messages or chat.

## More of the hub you can use

- **Skills**: `list_skills` returns the team's reusable instructions (e.g. "Product copy", "Instagram carousel").
  When one fits the task, follow it. `save_skill` adds or improves one for the whole team.
- **Inbox**: `list_inbox` shows files waiting to be sorted, with the hub's filing suggestion; `file_inbox_item`
  files one into its board with tags.
- **Canvases**: `list_canvases`, `get_canvas`, `create_canvas`, `update_canvas` lay out moodboards and layouts
  from hub files, notes, text, shapes and frames (see "Designing on a canvas" above). To rearrange, send
  `replace_items` with the `revision` you read.
## Designing on a canvas (flyers, posts, layouts)

People and agents edit the same canvas with the same pieces, so build it the way a designer would leave it for
the next person. The brand kit decides the look; the canvas only arranges it.

- **One page, one frame.** Put the design inside a `frame` with a real size (`a4`, `a5`, `letter`, `ig-post`, …).
  People export that frame as PNG, PDF or print PDF. Give the frame page guides
  (`grid: {margin, columns, gutter}`, fractions of the frame width) and place pieces on the margins and columns.
  The frame cuts off whatever hangs over its edge, on screen and in exports. For a full-bleed image, give it the
  frame's own `x, y, w, h`; the picture is cropped to fit, and `crop: {x, y, zoom}` chooses which part shows.
- **Type comes from the kit.** Use the kit's fonts as `family` and its colors as hex values; never pick a new font
  or color. If the kit has a "Text styles" section, set each text's `styleId` to the style it follows and copy that
  style's font, weight, line height, letter spacing and case onto the text. Size and color are yours to choose.
- **Start from a template when one fits.** `list_canvases` shows them; read it with `get_canvas` and build the new
  canvas from its items, changing words and images, not the layout.
- **Locked pieces are decisions.** Never move, restyle or remove an item with `locked: true`. If the task needs it
  changed, say so and leave it for a person.
- **Leave it editable.** Real text stays text (never baked into an image), one text box per thought, thin rules as
  1 to 2px shapes, related pieces sharing a `group`, and a short `name` on anything a person will look for in the
  layers list. Don't stack near-duplicates; `hidden: true` is for alternates you want kept.
- **Keep image fades editable.** Use the original transparent artwork and an asset item's `fade: {from, start, end}`.
  `from` is the visible edge or corner (`top`, `top-right`, `right`, `bottom-right`, `bottom`, `bottom-left`, `left`, `top-left`).
  `start` and `end` are fractions of the image's gradient line: `0 <= start < end <= 1`. Use `opacity` for edge strength.
  People can adjust these in the image's properties panel. Omit `fade` to remove it. Preserve existing fades when updating a canvas.
- **Facts come from the client profile.** Dates, prices, places and names must match `get_client_profile`. If two
  sources disagree, put a `note` beside the page saying what to confirm instead of choosing silently.
- **Generated images are artwork, not layout.** Use `generate_media` for the picture only, with no words in it, then
  set the type on the canvas.

## Design references (look before you design)

The hub keeps examples of what good looks like: layouts, typography, materials and finishes (gold, foil, paper,
marble), photography style, color and mood. They live in the `References` board and the boards under it, in the
client's workspace and in the shared Asala library.

- **Ask first.** Before laying out a page or writing an image prompt, call `find_references` with the look you are
  after (`"shiny gold"`, `type: "layout"`), and read the descriptions. Each one says what it is a reference for,
  what to take, and what not to copy.
- **Describe, do not copy.** Put what you learned into your own words in the prompt or layout ("polished yellow
  gold, mirror finish, hard reflections"). Never trace a layout or reuse someone else's artwork, photo or wording.
  Free material swatches (rights line says CC0) may also be passed to `generate_media` as `reference_ids`.
- **The brand kit wins.** A reference shows a direction; fonts, colors, logos and tone still come from the kit.
- **Never a deliverable.** A file with `reference: true` cannot go into a post or blog post, and must not be handed
  over as finished work.
- **Shared library files belong to another workspace.** A result marked `shared` cannot be placed on this client's
  canvas or passed to `generate_media` by its id. Copy it in first: `upload_asset` with `url` = its `download_url`,
  `board_path: "References/<section>"` and the same description, then use the new id.
- **Adding one.** File it under `References/<Layouts | Typography | Materials & finishes | Photography style |
  Color & mood>`, name it `ref-<type>-<subject>-<variant>`, and write five description lines: `Reference for:`,
  `Take:`, `Do not copy:`, `Source:`, `Rights:`. The board is what makes it a reference; a tag alone does not.

- **Generate**: `generate_media` makes images, edits, SVG, GIFs and video with the brand kit applied, saved into a
  board; check progress with `list_generations`. It spends the client's AI budget, so say what you will make before
  large batches.
- **Automations**: `list_automations` and `save_automation` set up recurring tasks (cron schedule, optional skill).
- **New client** (Asala staff, signed in to all clients): `create_workspace`, after confirming the name with the user.
- You cannot permanently delete anything or change people, API keys or security settings.
