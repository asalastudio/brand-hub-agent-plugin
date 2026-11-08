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
  download URL). Do not invent logos or recreate brand assets.
- New assets you produce (images, copy, documents) go back into the hub: `save_document` for writing,
  `start_upload` + upload + `finish_upload` for files. Put them in the board the user named, or the closest matching
  board (`board_path` like `Campaigns/Spring 2026`; missing boards are created).
- Set Status "In review" only when the user asks for review. You can never mark anything Approved: approval is a
  person's sign-off in the hub.
- Never write the hub API key into the repository, BRAND.md, commit messages or chat.
