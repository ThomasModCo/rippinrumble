# Rippin Rumble — changelog

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
