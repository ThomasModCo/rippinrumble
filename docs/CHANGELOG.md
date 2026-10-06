# Rippin Rumble — changelog

## v0.9 — 2026-10-06 — gallery by rarity

### Changed
- Gallery now opens on four tabs: Common, Uncommon, Rare, Legendary, each showing its found count. A tab shows every card in that rarity: found cards show their art and name, the rest show the card back with "Not found yet". Tapping a found card opens its details; closing returns to the same tab.
- The Gallery and Help buttons now stay on screen after a reveal, not only in the lobby. They are still hidden while a pack is ripping.
- The interface keeps clear of phone notches and rounded corners (safe-area insets).

### Includes
- The v0.8 fix: the card popup opens above the landed cards.

### Verified
- Three rounds, then each tab opened at 1100x680 and 390x800 (2x): card counts per tab 18 / 14 / 13 / 5, found cards match what was pulled, a found card opens its details and closes back to the gallery, and the gallery opens from the result screen. No console errors, no outside requests.

### Not changed
- The gallery still resets when the page is reloaded (session only), as decided for the first build.

## v0.8 — 2026-10-06 — popup above landed cards

### Fixed
- The card popup opened underneath the card that was clicked. The sharp landed cards added in v0.7 were stacked above the popup layer (a hovered card had a higher stacking order than the popup). The landed layer is now isolated below the whole interface.

### Verified
- Clicked each of the five landed cards in turn: the popup opened every time and no landed card was drawn over it. Screenshot read. No console errors, no outside requests.

### Packaging
- From this version every package is complete (index.html, the whole art_optimized folder, docs), so folders can be replaced safely.

## v0.7 — 2026-10-06 — maximum clarity once cards have landed

### Added
- When the five cards have landed, each finished card (art, frame, name) is resampled once to the exact number of screen pixels it covers, lightly sharpened, and laid over its 3D twin on the pixel grid. Nothing scales or filters it while it rests. It follows the 3D card for hover, and is resampled again when the card settles at a new size (hover lift, window resize). It fades out when the round ends.
- Higher-resolution card art: `art_optimized/cards_hd/`, 1200x1800, about 280 KB per card (14 MB for the set). Used for the 3D scene, the landed cards and the popup. The gallery grid keeps the lighter 800x1200 copies in `cards/`.
- While cards are landed the scene behind them drops the film grain, halves the vignette, turns bloom down and shows true image colours (no tone mapping, correct sRGB output).

### Trade-offs
- The foil shimmer on rare and legendary cards is hidden while they rest, because the landed image sits on top of it. It still plays during the reveal.
- Each round downloads about 1.4 MB of card art, up from about 0.4 MB.

### Settings
- `CONFIG.landedClarity` (master switch), `landedDom`, `landedSharpen` (0.9), `landedBloom`, `cardDir`, `cardTexScale`. `?sharp=0` still switches all clarity changes off for comparison.

### Verified
- Same card, same position, 2x pixel density: fine-detail measure 307 in v0.6, 505 in v0.7, 521 for the browser drawing the image file directly; brightness and contrast now match the direct image (52.7 / 35.7 against 53.4 / 35.5).
- Two rounds at 1280x760, one at 375x667 (2x): landed cards appear, sit at rest on the pixel grid, the popup opens from a click through them, and they are hidden again in the lobby. No console errors, no outside requests.

### Not verified
- Frame rate and memory on a real phone. The automated 5,010-round check was last run on v0.1; game logic is unchanged since, but it has not been repeated.

## v0.6 — 2026-10-06 — the real cause of the blurry cards

### Fixed
- The 3D scene was being drawn at 1x resolution and stretched to fit high-density screens. The post-processing composer ignores the screen's pixel ratio when it is given its own render target, which the prototype does. It is now told the pixel ratio explicitly, so the scene is drawn at the screen's real resolution (up to `CONFIG.dprCap`, 2x).

### Kept from v0.5
- Screen-sized card textures, 4x anti-aliasing, 1.5x rendering on standard screens. These were not the main cause; v0.5 on its own made almost no visible difference on a high-density screen.

### Verified
- Same card, same position, 2x pixel density: fine-detail measure (variance of the Laplacian over the card art) 101 in v0.5, 307 in v0.6, 521 for the browser drawing the image file directly. Compared from native-pixel crops. No console errors, no outside requests.

### Not verified
- Frame rate on a real phone. On a 2x screen the scene now draws four times as many pixels as the prototype did, with anti-aliasing on top. If it stutters, lower `CONFIG.msaa` or `CONFIG.dprCap`.

### Still different from the popup
- Cards in the scene are lit, tone-mapped and pass through bloom, vignette and grain, so they look slightly hazier and lower in contrast than the flat image.

## v0.5 — 2026-10-06 — sharper cards in the 3D scene

### Why cards looked softer in the game than in the popup
- The popup shows the image directly. In the game the same image is a texture on a 3D card: the full 805x1200 texture was shrunk by the graphics card using blended mipmaps (soft), the scene had no anti-aliasing (rough borders), and standard-density screens rendered at 1x.

### Changed
- Card textures are now pre-scaled to the size the card has on screen (times `CONFIG.cardTexOversample`, 1.3) with a high-quality resize, and drawn without mipmaps.
- 4x anti-aliasing on the scene (`CONFIG.msaa`), where the browser supports it.
- Standard-density screens render at 1.5x and scale down (`CONFIG.minPixelRatio`). High-density screens still render at up to 2x (`CONFIG.dprCap`).
- Card choice is repeatable when `?seed=` is set (test aid only).
- Adding `?sharp=0` to the address switches all three changes off, for comparing old and new.

### Not changed
- Math, provider, round flow, art files. Only `index.html` differs from v0.4.

### Verified
- Plays a round at 1280x760 at 1x and 2x pixel density with no console errors and no outside requests. Old and new compared on the same cards from screenshots: finer detail and cleaner borders in the new rendering.

### Not verified
- Frame rate on a real phone with the heavier rendering. If it drops, lower `msaa` or `minPixelRatio`.

## v0.4 — 2026-10-06 — card details popup

### Added
- Tap or click a revealed card to open a popup overlay with a larger framed image, the card name, rarity, type, faction and the full description from the "Chimera cards" sheet. Works from the gallery too (pulled cards only); closing returns to the gallery.
- A one-time hint, "Tap a card to read about it", shown after a reveal until the player opens a card.
- Card data file `docs/cards_chimera_2.json`: all 50 cards with level, type, faction and description (copied from the sheet on 6 October 2026).

### Changed
- Fonts: Roboto (Apache 2.0, bundled) for body text, meaning card descriptions and the text in the help, gallery and Coins sheets. Bebas Neue for card titles, including gallery captions. Orbitron stays on the interface: buttons, headings, labels, numbers.
- Opening a card during Autoplay stops Autoplay.

### Not changed
- Math, provider, round flow and art. Only `index.html` differs from v0.3.

### Verified
- Popup opened by a real click on a revealed card and from the gallery at 1280x760 and 375x667; closes with the X, the backdrop and Esc; no console errors, no outside requests. Layout read from screenshots at both sizes.

### Open
- Descriptions are shown as written in the sheet, including the references to existing franchises flagged earlier. Renaming is still deferred by the owner.

## v0.3 — 2026-10-06 — result box

### Changed
- Result box is now a plain translucent black box with slightly rounded corners. The cut-corner outline, which rendered with broken borders, is removed.

### Not changed
- Math, provider, round flow, art and the v0.2 card font. Only `index.html` differs.

### Verified
- Plays a round at 1280x760 and 375x667 with no console errors and no outside requests; result box read from screenshots at both sizes.

## v0.2 — 2026-10-06 — card font

### Changed
- Card names and rarity labels are drawn in Bebas Neue (SIL Open Font License, bundled in `index.html`). The interface stays in Orbitron. Sizes are tunables: `CONFIG.cardFont`, `cardNamePx`, `cardRarityPx`.

### Not changed
- Math, provider, round flow and art are identical to v0.1. Only `index.html` differs.

### Verified
- Loads and plays a round at 1280x760 with no console errors and no outside requests; card text read from a screenshot. The 5,010-round and 200,000-round checks were run on v0.1 and not repeated, since no game logic changed.

### Open
- Owner feedback: rare and legendary cards appear too often. This comes from the engine's deck make-up (see the concept doc); a decision on the math is pending.

## v0.1 — 2026-10-05 — first playable build (playtest, local provider)

Built from the prototype `Rippin Rumble – Pack Lobby.html`, the math config `rippinrumble_config_1.json` and the spec `rippinrumble_spec_1.md`.

### Added
- Result provider interface (`open`, `play`, `collect`) with `LocalProvider` wrapping the GnarOne Rip & Rumble module, preset `sheet-v4` (RTP 0.95), bundled unmodified. One marked line swaps the provider.
- Two play types: Base (5, 10, 25, 50 Coins) and Boosted (10, 20, 50, 100 Coins). Bundle pack shown locked ("SOON").
- Round flow: play -> rip -> five Chimera cards revealed lowest rarity first -> collect -> result panel with a line per rarity.
- 50 Chimera cards loaded on demand from `art_optimized/cards/`; card choice is cosmetic and follows the provider's rarity counts.
- Help sheet (how it works, Coins per card, set multipliers, disclosures), session gallery, out-of-Coins sheet with playtest reset, "Show me" sample rip, toast messages.
- Autoplay (10 rounds) and Turbo; music loop and generated placeholder sound effects with one sound toggle.
- Test hooks (`window.__QA`), `?seed=` for repeatable draws, `?dev=1` for the last provider response.
- three.js r128 and GSAP 3.12.5 bundled into `index.html`; the prototype's CDN loads are gone.

### Changed from the prototype
- Art is read from `art_optimized/` instead of being embedded, so the build must be served over http(s).
- "$" prices and "bet" labels replaced with Coin amounts and play wording.
- Lobby opens on Base. Prototype price tiers (0.5 / 1.0 / 2.5) replaced by the engine's play types.
- Portrait UI scale raised (basis 720 x 1280 instead of 980 x 1500) so touch targets are usable on phones; a few portrait button positions moved so the settings strip clears Turbo.
- Several legendaries in one pack now reveal one after another instead of overlapping.

### Departures from spec 1
- Coins are collected when the reveal ends, before the result panel shows; the balance panel shows Coins now, so the result panel has no separate "Coins now" line.
- One sound toggle instead of separate music and effects toggles (the settings art has one sound icon).
- Autoplay stop rule is a setting (`CONFIG.autoStopOn`: `legendary` | `big` | `none`). It ships as `legendary`, as agreed.

### Verified
- 5,010 rounds per play type through the running game: Coins shown, balances, single collect and card rarities matched the provider every round (0 mismatches).
- 200,000 rounds per play type against the provider: return 0.9524 Base, 0.9519 Boosted; 0 accounting mismatches.
- No outside requests, no platform script, no console errors at 900x640 and 375x667 (software rendering).

### Known issues / not verified
- A legendary appears in roughly a third of Base packs and two thirds of Boosted packs, so Autoplay's legendary stop fires within a few rounds and the long legendary reveal plays often.
- Sound not checked by ear; no run on a real phone or GPU yet.
- Card names close to existing franchises are still to be renamed before anything public.
- Bundles, Dawnbreak, Rico, deck select and a persistent gallery are not in this build.
