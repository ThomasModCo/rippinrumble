# Rippin Rumble v0.1 — build spec

Date: 2026-10-05. Source prototype: `Rippin Rumble – Pack Lobby.html`. Math config: `rippinrumble_config_1.json`.

## 1. Output

```
build/rippinrumble_v0.1/
  index.html                 code, fonts, UI button art, three.js, GSAP, math engine: all inline
  art_optimized/cards/*.webp 50 cards, 800x1200, loaded on demand
  art_optimized/ui/*.webp    background, 3 packs, card back, 4 frames (preloaded at boot)
  art_optimized/audio/chimera_loop.mp3
```

No request leaves the build folder. No SDK script, nothing from rollerz.dev.

## 2. Code blocks inside index.html (in this order, each clearly marked)

1. `CONFIG` — every tunable in one object (section 8).
2. `MATH` — the GnarOne Rip & Rumble module (`rumble-engine/src/rgs.js` and its imports) bundled unmodified into one global, `RipRumbleMath`. Nothing else in the file touches payout rules.
3. `PROVIDER` — the only thing the game talks to:
   - `open() -> { balance, currency, levels: { BASE:[...], BOOSTED:[...] } }`
   - `play(amountMinor, playType) -> { roundId, totalBetAmount, totalWinAmount, balance, nextAction, data }` where `data.cardPacks[0]` holds per-rarity `count`, `valuePerCard`, `multiplier`, `totalValue`.
   - `collect(roundId) -> { balance }`
   - `LocalProvider` implements these with `RipRumbleMath.createGame({ preset: 'sheet-v4' })` and a starting balance of 125000 minor units. All calls return Promises.
   - SDK swap point: one line, `const provider = new LocalProvider(CONFIG)`. The SDK build replaces that line and adds the script tag; nothing else changes.
4. `GAME` — round flow (section 3) and card selection (section 4).
5. `RENDER` — the prototype's three.js scene, pack rip, particles and post-processing, kept as is.
6. `UI` — DOM controls, sheets, result panel, gallery, audio.

## 3. State machine

`boot -> lobby -> placing -> opening -> revealing -> result -> collecting -> lobby`

| State | Enter | Exit |
|---|---|---|
| boot | load UI art and fonts, `provider.open()` | all loaded -> lobby |
| lobby | controls enabled | OPEN tapped and balance >= level -> placing |
| placing | controls disabled; `provider.play()`; balance set from response; choose 5 cards; load their 5 textures | resolved -> opening; error -> message sheet -> lobby |
| opening | prototype rip timeline | rip done -> revealing |
| revealing | 5 cards deal and flip in rarity order, lowest first | last flip + 0.4 s -> result |
| result | result panel; Coins count up over 0.6 s | CONTINUE tapped, or 1.8 s in Autoplay -> collecting |
| collecting | `provider.collect()`; balance set from response; cards added to gallery | resolved -> lobby |

Autoplay: runs 10 rounds; stops on a legendary reveal, when balance < level, or on a second tap. Turbo: timeline `timeScale` 2.2, result hold 0.9 s. Turbo never changes the result.

Insufficient Coins: OPEN is dimmed and a sheet offers "Reset Coins to 1,250" (playtest only).

## 4. Card selection (cosmetic)

From the provider's rarity counts, pick that many distinct cards of each rarity at random from `cards_chimera.json` (5 legendary, 13 rare, 14 uncommon, 18 common). Sort for reveal: common, uncommon, rare, legendary. The provider result is never altered by this step.

## 5. Screens

- **Lobby.** Prototype layout. Three pack slots stay so the carousel keeps its shape: Base (pack 1), Boosted (pack 2), and Bundle (pack 3) shown locked with the label "BUNDLE: SOON" and not selectable. Labels read "BASE" and "BOOSTED". The stepper shows the play level in Coins and steps through the selected play type's four levels only. Balance panel shows Coins.
- **Result panel.** Title "RETURNED", the Coin amount, net for the round, one row per rarity present (`2 x RARE  1.0 x2 = 4`), "COINS NOW", and CONTINUE. Sits in the lower third so the five cards stay visible.
- **Help sheet.** How it works (4 lines), the pay table per card and the count multipliers, a SHOW ME button, and the disclosures: No Purchase Necessary. 18+. Void where prohibited.
- **Show me.** At most 5 s: a scripted sample rip with fixed cards (1 rare, 1 uncommon, 3 common) and three captions. It never calls the provider and never moves the balance. Offered once on first load and always from Help.
- **Gallery.** In-page sheet, grid of all 50 cards grouped by rarity. Pulled cards show their art; others show the card back at 35% opacity. Counter "N / 50". Session only.
- **Settings.** The prototype's vertical strip: music on/off, effects on/off, help.

All sheets are in-page. No `alert`, `confirm` or `prompt`.

## 6. Money and copy

- Integer minor units everywhere in code. Display = minor / 100 with thousands separators and up to one decimal.
- Words used: play, Coins, redeem, returned. Never: bet, wager, cash-out, win/payout as nouns for money, RNG.
- OPEN label stays "OPEN".

## 7. Audio

Music: `chimera_loop.mp3`, starts on the first tap, 0.35 volume. Effects are generated with Web Audio (no files): rip (filtered noise, 0.5 s), card flip (short click), rarity stings (one rising tone per rarity, longer for legendary), Coin count-up ticks.

## 8. Tunables (`CONFIG`)

`startBalanceMinor 125000` · `levels.BASE [500,1000,2500,5000]` · `levels.BOOSTED [1000,2000,5000,10000]` · `preset 'sheet-v4'` · `autoRounds 10` · `autoHoldSec 1.8` · `turboScale 2.2` · `turboHoldSec 0.9` · `resultCountSec 0.6` · `revealStaggerSec 0.28` · `legendaryHoldSec 0.9` · `musicVol 0.35` · `sfxVol 0.6` · `dprCap 2` · `showMeSec 5`.

## 9. Test hooks

`window.__QA = { state(), select(i), setLevel(i), open(), continue(), last(), balance(), seed(n) }`. `?seed=N` makes the provider's draws repeatable. `?dev=1` shows the last provider response in a corner panel.

## 10. Acceptance

- Plays lobby to lobby with no console errors at 900x640 and 375x667.
- Over 5,000 scripted rounds per play type: Coins shown on the result panel equal `totalWinAmount`, and balance equals the provider balance after every `play` and `collect`.
- Measured return over those rounds is within sampling error of 0.95.
- No network request outside the build folder; no `sdk` or `rollerz.dev` string in the output.
