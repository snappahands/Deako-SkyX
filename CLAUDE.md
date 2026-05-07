# Deako + Vivint — Sky X Presentation

Cloned from `Deako-Maronda/index.html` 2026-05-07 with Sky X branding applied
and all builder-specific copy replaced with the `{Builder}` placeholder, ready
to be reskinned per prospect.

## Brand spec applied
- **Primary blue:** `#1493d1` (replaces orange-400 `#009844` in Maronda deck)
- **Light blue (gradient mid):** `#7bb0e7`
- **Deep blue / charcoal:** `#0e1827` (replaces navy-700 `#262927`)
- **Headline + body font:** `Centrano` (Centra No2) — pulled from skyx.com's
  Webflow CDN. Bold/Medium for headlines, `Centrano Book` for body. Falls
  back to Inter (Google Fonts) and the system sans stack.
- Source: `https://www.skyx.com/` and its shared Webflow CSS
  (`https://cdn.prod.website-files.com/65f45517ae0fc691f332b7a8/css/skyplug.shared.*.css`)

## Tailwind palette mapping

The Tailwind config still uses the names `navy` and `orange` for backwards
compatibility with the cloned class names. The hex values are now Sky X colors:

- `navy-*` palette = Sky X deep-blue scale, with `navy-700` = `#0e1827`
- `orange-*` palette = Sky X primary-blue scale, with `orange-400` = `#1493d1`

So `text-navy-700` renders as deep blue; `text-orange-400` renders as Sky X
primary blue. Don't rename the Tailwind keys without sweeping the whole HTML
— many class references depend on them.

## Builder placeholder
Every reference to "Maronda" / "Maronda Homes" is replaced with the
`{Builder}` token. When this deck is sent to a real prospect, do a single
find/replace from `{Builder}` → the actual builder name (or use it as-is for
generic Sky X-branded walkthroughs).

## What's preserved from the Maronda deck
- All Vivint product specs and pricing
- Slide structure, animations, navigation, feedback panel, capture button
- "Deako is the exclusive certified reseller of Vivint for the builder
  channel" core message

## What changed
- Title: "Deako + Vivint — Maronda Homes" → "Deako + Vivint — {Builder} (Sky X)"
- All visible "Maronda" / "Maronda Homes" copy → `{Builder}` token
- localStorage keys: `maronda-*` → `skyx-*` (so Sky X deck doesn't share saved
  edits/feedback/lock-state with Maronda deck)
- Window-scoped tracker function: `__marondaTrack` → `__skyxTrack`
- Slide 4 (Nate Amidon VP testimonial — Maronda-specific) removed; slides
  5–13 renumbered to 4–12. Total slide count: 12.
- Discreet `analytics →` link in the bottom-left corner alongside the version
  badge

## Special tracking — provisioned but OFF

The deck has its own tracking lane separate from the Maronda bin. It is
**disabled** until a fresh JSONBin is provisioned and constants are filled in.

To enable:
1. Create a new public JSONBin at https://jsonbin.io and seed it with
   `{"events": []}`.
2. In `index.html`, find the block near the `<!-- Recipient analytics -->`
   comment and replace:
   - `window.SKYX_SPECIAL_TRACKING_ENABLED = false;` → `true`
   - `window.SKYX_TRACKING_BIN = 'PLACEHOLDER_SKYX_BIN_ID';` → the new bin ID
   - `window.SKYX_TRACKING_KEY = 'PLACEHOLDER_SKYX_MASTER_KEY';` → the master key
3. In `analytics.html`, replace the matching `BIN` / `KEY` constants near the
   top of the IIFE.
4. Hard refresh the deck and confirm events flow.

While the flag is `false`:
- `window.__skyxTrack` is a no-op stub (calls don't blow up)
- The IIFE early-returns before fetch
- `analytics.html` shows an empty state because the placeholder bin ID
  resolves to a 404 — no real bin is touched.

## Repo / git
This directory is **untracked** in the parent `ClaudeProjects` repo (matches
the Maronda pattern — that one was moved to its own repo). When ready to ship,
init this as its own repo and push to `github.com/snappahands/Deako-SkyX`
(GitHub Pages then serves it at `https://snappahands.github.io/Deako-SkyX/`).
