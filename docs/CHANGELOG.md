# Rippin Rumble — changelog

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
