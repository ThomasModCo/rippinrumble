# Rippin Rumble — changelog

## v0.20 — 2026-10-27 — result box, ladder bar, help in the corner

### Changed
- Result box, redesigned: the amount returned sits on the left, stacked (RETURNED, the amount, then a NET pill); the rarity breakdown sits on the right in aligned columns behind a thin divider. Everything stays inside the box at every screen size; phones get the same layout.
- Climb the ladder is now a bar along the bottom of the result box, with a crossed-swords icon to show it is a fight. It only appears when the round returned Coins; otherwise the box is shorter.
- Help sits in the top right corner, lined up with settings below it. Change deck (lobby only) sits just to its left. RICO has no help button art, so Change deck keeps the corner there.

### Fixed
- The result box overflowed on every desktop size since v0.19 (the amount spilled out on the left, the ladder button on the right).
- A round is collected and shown only once even if the reveal finishes twice. This could show "Could not collect this round" in automated tests; no player action triggers it, but it is now guarded.

### Verified
- No overflow at 1024x768, 1280x720, 1512x982 and 1920x1080; help and settings share the same right edge. Phone (390x844) checked with the ladder bar. Ladder flow on desktop and phone, and 120 rounds per deck with no mismatches. No console errors.

## v0.19 — 2026-10-27 — power on every card, and the Ladder (showcase)

### Added
- Power: every card shows its power level from the sheets (the Lvl column, 1 to 16), in an emblem drawn in its deck's own style: a bevelled metal plate with the frame's chamfered corners on Chimera, an art-deco gilt diamond on Dawnbreak, a neon tube sign on RICO. The emblem takes the card's rarity colour and the number is centred on its visible shape. The same emblem appears on the 3D cards, the gallery, card details and the Ladder screen; card details also list "Power N".
- The Ladder (showcase): after a round that returns Coins, "Climb the ladder" puts those Coins on one of the five cards just pulled. The champion fights three cards, one at a time, each stronger than the last (about power 5, 10 and 15). Win and the Coins multiply; lose and they are gone; after any win the player can take the Coins instead.
  - Every fight is even on average: the multiplier is 1 / win chance (from 1.2x to 10x), so the Ladder never changes the game's return. A stronger champion wins more often but climbs in smaller steps, so finding a legendary does not mean winning more.
  - The most a ladder can return is 500x the play amount.
  - Fight screen: champion against opponent, a win/lose bar sized to the real chance, a needle that lands on the result, and win, loss and final-win sounds.
- Card data files: docs/cards_chimera_4.json, docs/cards_dawnbreak_3.json, docs/cards_rico_3.json now carry power (earlier files kept).

### Showcase only
- Fight results are decided in the game for now, not by the engine. Before release the three Ladder calls (start, fight, take) need to move to the GnarOne engine and be approved by Rollerz, along with the cap and the wording.

### Verified
- Ladder over 20,000 ladders per champion power: power 2 returns 1.03 per Coin, power 9 returns 1.00, power 16 returns 1.00 (full climbs 0.8%, 8.4% and 32.3%).
- Full flow on desktop and phone: Coins leave the balance when the ladder starts, wins and takes add up, a loss ends the ladder. The Ladder screen fits on 1280x720 and 390x844. No console errors.

## v0.18 — 2026-10-19 — 25 new cards per deck, remade RICO art

### Added
- Chimera: 25 new cards (75 in all): 1 legendary, 4 rare, 10 uncommon, 10 common.
- Dawnbreak: 25 new artifact and spell cards, plus Thalvorn, who is now in the Dawnbreak sheet (76 in all).
- RICO: 25 new cards (75 in all).
- Card details show extra facts from the lore where the sheet has them: who wields an artifact, the school of a spell, and who a spell is associated with.

### Changed
- RICO: all 50 original cards use the remade art (the -v3 renders).
- Card names, titles, rarities and lore now come from the updated sheets for every card, old and new. Some existing Dawnbreak and RICO lore changed length because the sheets changed; the sheets are the source.
- Card details now show the card's title next to its rarity in every deck (Chimera: title and faction; Dawnbreak: title and race or card type; RICO: title and type).
- Card data files: docs/cards_chimera_3.json, docs/cards_dawnbreak_2.json, docs/cards_rico_2.json (earlier files kept).

### Unchanged
- Math: the engine returns rarity counts, so larger decks do not change what a pack returns or how often each rarity appears.

### Verified
- Every card in the three sheets has art in both sizes and matched lore (no card falls back to its summary); no image without a card.
- 24 scripted rounds per deck: 0 mismatches. Card details checked on desktop and phone for one new card per deck. No console errors.

## v0.17 — 2026-10-12 — phone layout, round 2

### Changed (phones in portrait; desktop unchanged)
- Top bar: menu button top left (Gallery is now in the menu), balance top right, logo between them. Coins fly up into the balance.
- Play amount (minus, amount, plus) is centred directly above OPEN.
- Reveal: the cards gather face down, then come forward one at a time, full size, lowest rarity first. Each flips, holds, and goes onto the pile; legendaries get the light, flash and longer hold.
- When all five are out they fan into a stack: the best card on top, the others peeking out to the left and right. Swipe left or right to turn the stack; tap a peeking card to bring it to the top; tap the top card to read it. Dots under the stack show where you are, coloured by rarity.
- Card details: the art fills the width of the screen. Swipe left or right to move through the round's cards (or, from the gallery, the found cards on that rarity tab); a counter shows "2 / 5". Arrow keys do the same on desktop.

### Fixed
- The legendary sparkle asked for a bell overtone above what audio can play; that overtone is now skipped.

### Verified
- 360x780 and 390x844, Rico and Dawnbreak: no overlapping or off-screen controls; the top card after the reveal is the last (best) card; swipe turns the stack; tapping opens the top card; card details step through 5 cards from the reveal and 12 found commons from the gallery, and closing returns to the gallery.
- Desktop measured the same as v0.16. 16 scripted rounds per deck in both layouts: 0 mismatches. No console errors.

### Not verified
- Not run on a real phone. Opening a pack on a phone now takes about 7.8 s from tap to stack with no legendary (about 2.5 s longer than desktop), plus about 0.9 s per legendary; Turbo makes it 2.2 times faster.

## v0.16 — 2026-10-12 — phone layout

### Added
- Phones in portrait get their own layout instead of a shrunk desktop one:
  - Top bar: Gallery on the left, a small logo in the middle, a menu button on the right.
  - Control panel at the bottom, in thumb reach: balance and play amount (minus, amount, plus) on one row; Autoplay, OPEN and Turbo on the row below, on a dark gradient.
  - Menu (new): Change deck, Gallery (with cards found), How it works, Sound on/off, and the No Purchase Necessary line. It replaces the corner settings strip, the help button and the CHANGE DECK button on phones.
  - Packs: swipe left or right on the pack, or tap the arrow buttons beside it. Only the chosen pack's name is shown, so nothing is cut off at the screen edges. The pack is larger and sits higher.
  - Cards are revealed in a 3 + 2 grid (lower rarities on top, the best two below), all fully visible and easy to tap, instead of five overlapping cards in a row.
  - Result box sits between the lower card row and the balance.
  - Help, gallery, card details and the menu slide up from the bottom of the screen as sheets, with 48 px close buttons.
- Every button on the phone layout is at least 44 x 44 px. Sizes are set in pixels, so they no longer shrink with narrow screens.

### Unchanged
- Desktop and landscape layout: measured the same as v0.15.

### Verified
- 360x780, 390x844 and 430x932, all three decks: no control under 44 px, no controls overlapping, nothing off screen; swipe changes the pack; the menu opens the deck screen. 16 scripted rounds per deck: 0 mismatches. No console errors.

### Not verified
- Not run on a real phone. Phones held sideways still get the desktop layout scaled down.

## v0.15 — 2026-10-12 — Coins fly into the balance

### Added
- When a round returns Coins, Rollerz coins fly from the returned amount on the result box into the coin on the balance. More coins for bigger returns (4 for a small return, up to 22 for the top returns); none when nothing is returned.
- Each coin plays a metallic clink as it lands, rising slightly in pitch through the run. The balance flashes and its number steps up with every coin, then a closing shimmer plays when the last coin lands.
- The balance glows while the coins come in, in the deck's colour (cyan Chimera, teal Rico, gold Dawnbreak).
- Turbo makes the flight about twice as fast. Autoplay waits for the last coin before opening the next pack. CONTINUE during the flight finishes it at once with the correct balance. With reduced motion turned on, there is no flight: the balance glows and the shimmer plays.

### Changed
- The bell cascade that played when the result box appeared is replaced by the coin sounds.
- The coin picture is embedded in index.html, so this update needs no new art files.

### Verified
- Balance shown after the flight equals the provider balance; no coins left on screen after CONTINUE mid-flight. 16 scripted rounds per deck: 0 mismatches. No console errors.

### Not verified
- Not heard by ear. Not run on a real phone.

## v0.14 — 2026-10-07 — button states preloaded

### Changed
- Every button picture for a deck (normal, hover, pressed, on, settings open) is fetched and decoded while the deck loads, and held in memory. Before, the hover and pressed pictures of Dawnbreak, Rico and the main button were fetched the first time they were needed.

### Verified
- In each deck, hovering and pressing every button and switching every toggle made no new image request. No console errors. 16 scripted rounds per deck: 0 mismatches.

## v0.13 — 2026-10-07 — one font, fixed sizes, layout fixes

### Changed
- Bebas Neue is the interface font in every deck (long text stays in Roboto). Orbitron, Jockey One and Gentium Book Basic are no longer in the build.
- Interface boxes are the same size and position in every deck; each deck only supplies its pictures. The main button is the same width everywhere and centred on the same line.
- Result box: one fixed size, centred midway between the bottom of the cards and the top of the main button.
- CHANGE DECK moved to the top right corner; the help button sits beside it.
- Balance: Dawnbreak uses its new background with the coin; the value is vertically centred in the bar in every deck.
- Chimera and Rico cards: name and rarity moved up 16 px so the rarity line is the same distance from the bottom edge as on Dawnbreak.
- Dawnbreak card popup: the frame is no longer cropped at the top and bottom.

### Verified
- Card size and position measured in all three decks: identical (within 0.2 px at 1280x720).
- Result box measured in all three decks: same size and position, equal space above and below (Dawnbreak's main button art is a little taller, so the space below is 5 px less there).
- 50 scripted rounds per deck: 0 mismatches. No console errors.

## v0.12 — 2026-10-07 — Dawnbreak and Rico decks

### Added
- Deck screen shown first (Chimera, Dawnbreak, Rico) and a CHANGE DECK button in the lobby. Coins and play levels carry over when the deck changes.
- Dawnbreak: 50 cards, own background, packs, frames with name ribbons, buttons and music loop; interface font Gentium Book Basic. Popup shows title, race, artifact and the full lore.
- Rico: 50 cards, own background, packs, frames, buttons and music loop; interface font Jockey One. Popup shows type, level and the description with design notes removed.
- `?deck=chimera|dawnbreak|rico` opens a deck directly.

### Changed
- The main button now uses the text-less button art in every deck; the game writes OPEN, CONTINUE or STOP (while Autoplay is running) on it.
- The gallery is tracked per deck. The sample rip uses cards from the current deck.
- Long one-block card descriptions are split into short paragraphs in the popup.
- Art layout: Chimera stays in `art_optimized/`; the new decks are in `art_optimized/dawnbreak/` and `art_optimized/rico/`. The card back is shared.

### Unchanged
- Math (GnarOne Rip & Rumble, `sheet-v4`), layout, card back and sound effects are the same in every deck.

### Verified
- 90 scripted rounds per deck (Base and Boosted, all play levels): Coins shown, balances and card rarities matched the provider every round (0 mismatches). All 150 card images load in both sizes.
- No outside requests and no console errors at 1280x720 and 390x780 (software rendering).

### Known issues / not verified
- Rico has no help button art; Help opens from the info icon in the settings strip.
- Card art not in a roster is left out: Dawnbreak Thalvorn; Rico golf cart and "ONE" courier van.
- Not run on a real phone; music change between decks not checked by ear.
- Rare and legendary frequency is unchanged (math review deferred).

## v0.11 — 2026-10-06 — cinematic sound effects

### Changed
- All sound effects replaced. The square-wave beeps are gone; each effect is now layered from filtered noise, sub-bass, detuned oscillators and bells, through a shared generated reverb and a compressor. Still no audio files apart from the music loop, which is unchanged.
  - Pack rip: rising whoosh, paper crackle, low rumble. Pack burst: bass impact with a long tail.
  - Common: card whoosh and muted thud. Uncommon: adds a warm two-note swell and a bell. Rare: impact, rising major chord, bells. Legendary: brass-like swell, sub drop, choir pad, bell sparkle (about 3.6 s).
  - Coins returned: a short bell cascade. Buttons: a soft tock.
- Impacts carry a mid-range layer so they are still heard on phone speakers, which cannot reproduce the deep bass.
- `CONFIG.sfxVol` is now 1.0 (was 0.6).

### Verified
- Each effect rendered offline and measured: none clips (loudest peak 0.87 of full scale, the legendary reveal), and loudness steps up with rarity (peaks 0.21, 0.37, 0.40, 0.87). Three rounds played with sound on, no errors.

### Not verified
- How they sound. They have been measured, not listened to. `rippinrumble_v0.11_sound_preview.mp3` plays all of them in order for review.

## v0.10 — 2026-10-06 — gallery: one row of six, arrows

### Changed
- The gallery popup is a fixed size and shows one row of six cards per page, with previous and next arrows at the sides and "Page 1 of 3" underneath. Left and right arrow keys and a swipe also turn the page. Each tab remembers its page.
- Gallery cards are larger: 195 px wide on a 1440 px window, up from about 124 px (57% larger). They scale down on narrower windows.
- Card names and "Not found yet" are centred under each card.
- On phones in portrait the same six cards per page are laid out as two rows of three, because six across would be too small to read.

### Verified
- Paging on every tab at 1440x820 and 390x800 (2x): 3 pages for Common, Uncommon and Rare, 1 for Legendary; arrows switch off at the ends; popup size stays the same on every tab and page. Screenshots read. No console errors, no outside requests.

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
