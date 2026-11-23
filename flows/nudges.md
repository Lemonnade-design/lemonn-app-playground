# Flow: Overtrading nudges (playground) (`nudges`)

> One self-contained file for converting this flow to another platform. Read https://lemonnade-design.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `ed10506` (components, tokens and rules: https://lemonnade-design.github.io/lemonnade-v3-docs/llms.txt).

Journey-aware P&L limit and TP/SL prompts: silent help on the order pad, and one nudge sheet when the revenge flag and the journey engine agree.

Prototype: https://lemonnade-design.github.io/lemonn-app-prototype/#/nudges · machine-readable: https://lemonnade-design.github.io/lemonn-app-prototype/flows/nudges.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `nudges-01` | Order pad · silent help | [8671:18433]() | — | The default — no interruption. The F&O commodity order pad (fno-25) as it is, plus silent help: “Set TPSL” becomes a TP/SL row, pre-filled and on (Target +20% · Stop-loss −10%); “Set a loss limit for today” is the first line of the dock. Buy fires the nudge picked in the settings (Case, Data state), once: the shared cap is 1 per session. |
| `nudges-02` | Nudge · from settings | — | — | The one nudge sheet — the risk meter — filled by the case and data state picked in the settings. Pills on the floating switcher: Meter (4f Soft dial · 4g Blocks), Risk level, Meter layout, Meter fill, Case, State. |
| `nudges-03` | Nudge · generic | — | — | Fires on Positions with the revenge flag when no single behaviour stands out, or the trader is too new to compare (state D). The meter and its own fact under it. Dismiss says just “Not now” (no order in hand). |
| `nudges-04` | Nudge · size-up · A (own loss) | — | — | Size-up after a loss, own history and last loss (A): the meter at high risk, “You are placing a bigger order right after a loss”. |
| `nudges-05` | Nudge · orders today · B | — | — | Orders today, own history (B): the meter, and the fact “3X more orders than usual today” under it. |
| `nudges-06` | Nudge · orders today · C (thin history) | — | — | Too few past sessions for “your usual”: the fact under the meter is today’s raw fact (“14 orders today”), no multiple. |
| `nudges-07` | Nudge · averaging down (TP/SL first) | — | — | Averaging down: the main button is TP/SL; the fact under the meter is the case\ |
| `nudges-08` | Set today’s loss limit | — | — | From the nudge’s “Set a loss limit for today” or the order pad dock. Pre-filled ₹25,000, quick amounts as pill tabs. Set limit → back to the pad with the limit shown. |
| `nudges-09` | TP/SL for this order | — | — | From the nudge’s “Add TP/SL to this order” or the TP/SL row. Target and stop-loss in %, the exit price under each. |
| `nudges-10` | Order pad · limit set | — | — | Silent help once a limit exists: “₹25,000 left of today’s loss limit” with a bar in the dock (tap to change it). |
| `nudges-11` | Order pad · close to the limit | — | — | ₹21,000 of ₹25,000 used: the bar and the line turn warning. Still no interruption. |

## Spec (README.md)


> PLAYGROUND, not a shipped flow. The product team's draft (2026-10-30, `playground/overtrading-nudges/README.md`),
> redesigned in L3 so the idea can be shown and tested. No Figma yet. Every number in the copy is an example; the real
> values are computed per trader.

- **Prototype:** `#/nudges` · single screen: `#/nudges-02` (the nudge, driven by the settings)
- **Status:** 🟨 exploration · last updated 2026-10-30
- **Entry:** the F&O commodity options order pad (`fno-25`, copied into this flow) · **Exit:** back to wherever the order pad came from

### The idea in one line

When an F&O trader starts chasing a loss, the app shows one short sheet at the moment of the order, with one fact
about today and one reason, and offers a loss limit for the day or a TP/SL on this order. He can always skip it.

### Screens

Each screen is the order pad in one state; within a screen, sheets swap over the same page. Screens 03–09 open with
their own case; changing Case or Data state in the settings switches the open sheet to the pick on any screen.

| Id | Screen | What it shows / when |
|---|---|---|
| `nudges-01` | Order pad · silent help | Default. TP/SL on and pre-filled, "Set a loss limit for today" in the dock. Buy fires the nudge from the settings, once. |
| `nudges-02` | Nudge · from settings | The nudge sheet for any case (17) × data state (A / B / C) and the button variant. |
| `nudges-03` | Nudge · generic | Fires on Positions: nothing stands out, or the behaviour can't be scored (state D). No bars. |
| `nudges-04` | Nudge · size-up · A | Bars: the trade he lost vs this order; line = his own last loss (red strip). |
| `nudges-05` | Nudge · orders today · B | Bars: his usual day vs today; line = the group (7 in 10 lost more). |
| `nudges-06` | Nudge · orders today · C | No "usual" yet: headline = today's raw fact, no bars. |
| `nudges-07` | Nudge · averaging down | A TP/SL-first case whose fact isn't a multiple: no bars. |
| `nudges-08` | Set today's loss limit | Amount field + quick amounts. |
| `nudges-09` | TP/SL for this order | Target / stop-loss % with exit prices. |
| `nudges-10` | Order pad · limit set | "₹25,000 left of today's loss limit" + bar. |
| `nudges-11` | Order pad · close to the limit | Bar and line in warning. |

### Navigation

```
nudges-01 ──Buy (1st time)──▶ nudge sheet ──Not now, place order──▶ order placed (toast)
                                 ├──Set a loss limit──▶ nudges-08 ──Set limit──▶ nudges-10
                                 └──Add TP/SL──────────▶ nudges-09 ──Save──────▶ pad, TP/SL on
nudges-01 ──Buy (again)──▶ order placed (cap: 1 nudge per session, 2 per day, shared by every nudge)
           (prototype setting "Show on every Buy", on by default: the nudge fires on every Buy / Sell, to demo it; off = the real cap)
```

### The nudge sheet — the risk meter (one template, every case)

Cut down on 19 Nov to the versions in use: Layouts 1 (bars) and 5 (risk bar, 5a–5g) and meters 4a–4e are removed (in
git history). It opens on a trader who is mid-order and stressed: what this is, a meter that shows how far from his
usual he is, one plain line on what he is doing, then the guardrail or Not now. Everything is centred; 24 between
blocks, the body text 16 under the meter, 32 from the last text to the dock's line (L3's sheet has 24).

| Slot | Content |
|---|---|
| Heading | "You are overtrading" (`heading-24`) over "Your chances of losing money today are high" (`label-14`, secondary) |
| Meter | 4f Soft dial or 4g Blocks (below); in the middle the level word — Safe · Caution · High risk — only (no multiple in any layout since 20 Nov) |
| Body text | Size-up: "You are placing a bigger order right after a loss"; every other case its own fact ("3X more orders than usual today"; state C: the raw fact, "14 orders today"). `description-14`, secondary |
| Dock | `ButtonGroup` vertical: `primary` = the case's guardrail · `secondary` "Not now, place order" (on Positions: "Not now"). Setting "Buttons → Both guardrails": primary · secondary (other guardrail) · ghost Not now |

**Settings** (prototype; also pills on the floating switcher):

| Setting | Values |
|---|---|
| Meter | **4f · Soft dial** (default) · 4g · Blocks · 4h · Speedometer · 0–100 · 4i · Speedometer · Low–High (4h / 4i have their own layout; Meter layout doesn't apply) |
| Risk level | **High risk** (default) · Caution · Safe — where the reading stops, with that level's colours, glow, grains and word |
| Meter layout | **Text first** (default: heading, sub line, meter, body text) · Meter first (meter, "You are overtrading", body text; other cases add "Your chances of losing money today are high" to their fact) · Tag first (a red secondary L3 Tag "Overtrading Detected" with a filled warning icon — 8 padding, 2 icon → text, an L3 gap — then "Your chances of losing money are high" (`heading-18`), the meter, body text) · With multiple (now the same as Text first: the dial's multiple was removed, 20 Nov) |
| Meter fill (Soft dial only) | **One colour** (default: the whole ring turns green → yellow → red as the needle moves; levels 0–45 / 45–65 / 65–100 %, high risk lands at ~75 % for 2X) · Blended (each part keeps the colour it was filled with, melting into the next over a soft band; levels 0–33 / 33–60 / 60–100 %, high risk at ~85 %) · Blended · 3 blocks (20 Nov: the blended ring cut by a 6 gap at each colour change — three blocks, each in its own colour) · 3 blocks · Low–High (20 Nov: the 3-block dial in three equal blocks (0–⅓–⅔–1; low stops at ⅙, medium at ½), drawn at 0.84 about its base centre, with Low and High beside its ends and Medium over the top — Heading/16, content/primary — the risk Tag in the disc where the level word was, fading in as the needle reaches the reading; under the dial the speedometer's heading and fact, whatever the Meter layout) · One colour · Low–High (20 Nov: the one-colour ring with everything of 3 blocks · Low–High — equal thirds, the scale round it, the tag in the disc, the heading and fact under it) |

**4f · Soft dial** (`sheets/SoftDial.tsx`, from the user's reference): a thick ring filled from the left with a radial
gradient (deep inside, light outside), sparse white grains (brightest in a soft wedge behind the needle; the ones on
passed levels fade away), small ticks, a solid black needle standing on the centre disc, glass on the fill (sheen, soft
highlight, fine rim), a faint tint on the rest, a blurred glow behind each level as the needle reaches it and a bloom at
the tip; the lower ends fade into the sheet. Before the needle moves the ring is plain grey. Motion (Web Animations,
one path for needle, fill, grains, colours and word): ease-in-out a little past the reading, back, forward a touch,
settle (≈ 2.7 s); colours blend in oklab; the word swaps Safe → Caution → High risk (out, then in).

**4h / 4i · Speedometer** (`sheets/GaugeMeter.tsx`, Figma LM → F&O Playground 6917:62779, 20 Nov): the Figma
speedometer illustration (`sheets/gaugeArt.ts`, its exported paths and colours — a brand illustration, not tokens): six
colour segments green → red with a thin pastel rim, a black ring with ticks, a dark face and a white needle; its scale
in Heading/12 — 4h 0 · 30 · 50 · 80 · 100, 4i Low · Medium · High — placed as in Figma. Its own layout: a pink wave (the
level's light tint, blurred 11.3) across the top of the sheet, the dial, 16, the risk Tag (md, secondary, loss / warning
/ profit, warning icon), 16, "Your chances of losing money is high" (Heading/20; medium / low for the other levels; Figma said "Your portfolio is at high risk", changed 20 Nov), 8, the fact (Description/14, secondary — Figma had Label/12, changed 20 Nov; size-up:
"You are placing a bigger order right after loss"); 24 from the sheet's top and 24 to the dock's line. Where the needle
stops: high risk 78 (as drawn in Figma), medium 50, low 15. Motion, once on open: the segments light up left → right
(0.26 s each, 70 ms apart) with their labels, then the needle swings up from 0 with the soft dial's run (eased, a little
past the reading, back a touch, settle; 2 s after 0.32 s) and the segment it lands on brightens once. Reduced motion:
the end state. Dark: the illustration as it is; the wave and text follow the theme.

**4g · Blocks** (`sheets/BlockDial.tsx`, from the user's reference): 13 thick rounded blocks with gaps, grey at rest; a
fast eased fill (1.8 s, the same run with overshoot and bounce) lights them in their level's colour with a gradient (no
glass); the block under the fill's head lifts outward and settles back — a wave travelling with the fill; level glows and
a tip bloom as on 4f.

**Mobile:** each dial's drawing area reaches past the ring (16 at the sides — the sheet's edge — and 70 above), pulled
back with negative margins so the dial doesn't move: mobile Safari clips SVG at the svg's box, which cut the blurred
glows off in hard edges. The text is drawn above the glow.

**Floating switcher (prototype only):** on the order pad and over the open nudge, a draggable pill: grip · Meter · Risk
level · Meter layout · Meter fill · Case · State. Each opens its own menu. `components/SheetSwitcher.tsx`, options in
`options.ts`. Not part of the design: don't build it.

**Vibration:** two short pulses (70 · 60 · 70 ms) when the sheet opens, not when it changes; setting "Vibrate when it
opens" (on). In the Lemonn Android app the page calls `window.LemonnApp.vibrate`; in a browser, `navigator.vibrate`
(Android Chrome, right after a tap; iPhone has none). Real app: a native warning haptic.

Data states: A = own history and last time lost · B = own history, no loss · C = thin history (raw fact, no multiple)
· D = can't score → the generic sheet. Cases: `data.ts`; words: `copy.json` `case.<id>.*`.

### Data

`data.ts`: `nudgeCases` (id, kind, `where` order | positions, primary guardrail `pnl` | `tpsl`, `ratio`,
`othersLostMore` %, `lastLoss` ₹, `othersVs`), values from the product team's compare file (2026-11-01); the sample
order (`orderContract`, `funds`), pre-filled TP/SL (20% / 10%), loss-limit presets and sample loss. The API must
provide, per nudge: case id, data state, the headline numbers, ratio, own last loss (A), group share.

### Rules & behaviour

- Silent help first: the sheet only appears when the revenge flag and the journey engine agree; capped 1 per session,
  2 per day, shared across all nudges.
- Every sheet is skippable in one tap; it never blocks the order, never asks for a deposit, never gives advice.
- Box 2 describes what happens to people, never "because you…". Never promise a green day. Never use "only 2 in 100
  stop" as copy.

### Differences from the draft

- **One consequence, not two:** the draft shows "Others who did this 67% lost more" and "What it cost them ₹2,500
  more" (or "Loss grew 1.8X"). The sheet keeps one line; the cost stat is dropped. Percentages read as "7 in 10".
- **"Overtrading Detected" is a tag** over the fact, not the headline: the tag says what this is, the fact says why.
- **Behaviour box → bars:** "3X more orders than usual" is drawn as his usual vs today, so the size is seen, not read.
- **Buttons:** one guardrail + Not now by default (one clear choice); the draft's two guardrails are the "Buttons"
  setting. Main button L3 `primary`, not lime (lime is Buy, right under this sheet).
- **"Not now" says what it does:** "Not now, place order"; on Positions just "Not now".
- **Not measured → left out:** cases the draft marks "Not measured" show no group line instead of the words.
- **Positions:** generic, expiry and giving-back fire on Positions in the draft; the prototype shows them over the
  order pad (no Positions page here yet), with the Positions dismiss label.

### L3 gaps

- No illustration for "slow down" / caution moments (the draft wants the Lemonn character).
- BottomSheet has no non-dismissible mode: without `onClose` a backdrop tap does nothing, but the sheet still follows a drag (5d pins it with a flow CSS rule on the dialog). A `dismissible={false}` prop would cover it.
- No static (non-tappable) pill: Layout 1’s Chip header is drawn in the flow (NudgeSheet .chip) in the pill-tab shape with error-light tokens. A Tag with `radius-full` and a larger size would cover it.

### Changelog

- 2026-10-30 · first build on the `playground` branch.
- 2026-10-30 · nudge sheet revamp: one card (big number + proof, 10 people filling in), pulsing icon, primary + secondary dock; tags, bars, explainer and disclaimer removed to cut text.
- 2026-11-01 · nudge sheet redesigned from the product team's 17 use cases for a one-glance read: ✋ + "Overtrading" tag, the fact as the heading, usual-vs-now bars, one consequence line; case values from their compare file; "Sheet title" setting removed.
- 2026-11-01 · settings: "Show on every Buy" toggle (fires the nudge on every Buy / Sell); data state is a dropdown.
- 2026-11-01 · "Layout" setting: 1 · Centred + bars, 2 · Big number, 3 · Compact alert.
- 2026-11-01 · tag reads "Overtrading Detected" (md), in every layout and case; Compact icon centred.
- 2026-11-01 · the Case / Data state settings now change the sheet on every nudge screen, not only 01–02.
- 2026-11-01 · the phone vibrates when the nudge opens (setting "Vibrate when it opens").
- 2026-11-01 · "Show on every Buy" is on by default.
- 2026-11-01 · floating Layout · Case · State switcher over the open nudge (prototype only, draggable).
- 2026-11-01 · the floating switcher shows on the order pad too, before the nudge fires.
- 2026-11-02 · multipliers are written with a capital X (2X, 3X) in every layout, so the sign sits level with the number.
- 2026-11-02 · every sheet in this flow has 24 above its content as well as below (L3 has 16 on top); a difference from L3.
- 2026-11-02 · Layout 1 header: Chip (✋ + label in one light-red pill, no circle; default) or Icon + tag — setting and floating switcher.
- 2026-11-02 · Chip header: 24 above and below it; on open it pops in as ✋ alone, then opens to show "Overtrading Detected" (while the bars grow).
- 2026-11-02 · a nudge with no bars and no consequence (giving back) keeps 24 under the headline, above the dock.
- 2026-11-02 · Size-up after a loss is the default case and heads the case list. Floating switcher: every control is a dropdown pill with its own designed menu (no system picker); the case pill truncates so the bar never runs off the screen.
- 2026-11-04 · Layouts 4 · Risk meter and 5 · Risk bar: where the trader is on a safe → danger scale, his usual vs now, animated once.
- 2026-11-04 · Layouts 4 and 5 trimmed: the meter / bar at the top, one heading, the tag under it; the not-safe line, the consequence and the bar's card removed.
- 2026-11-05 · Layout 5 is the default and has one version (size-up); the switcher hides Case and State on it.
- 2026-11-05 · Layout 5: zones renamed Low / High risk zone; the Usual and You now pills, the green usual dot and the tag removed; one floating tooltip ("High chance of losing money", two lines) centred on the red dot, sliding with it; heading "Overtrading, you just entered the high risk zone".
- 2026-11-05 · Layout 5: the tooltip grows out of the red dot once it settles (no longer rides along), with 12 of room above so its overshoot isn't clipped; the red dot then glows on loop like a live indicator (a soft red ring swells and fades). Reduced motion: all still.
- 2026-11-05 · Layout 5: the tooltip is one line, with its arrow near its right end (on the dot), so it covers the empty part of the bar on the left; it still grows out of the arrow.
- 2026-11-05 · Layout 5: the tooltip emerges from the red dot: a small red blob leaves the dot, rises to the arrow spot and opens into the dark bubble (0.7 s), text last.
- 2026-11-05 · Layout 5b · Everyone vs you, picked in the Case pill: Everyone and You tooltips in turn, Safe zone / trading to plan · Danger zone / chasing losses.
- 2026-11-05 · Layout 5c · Why first: L3 lg header "You're overtrading" · "Your chances of losing money are high", then the Everyone vs You bar without the zone notes.
- 2026-11-05 · Layout 5d · Story: 5c played as a short feature video (heading stamps in centred, the risk line, both rise, the bar builds step by step, buttons last); can't be dismissed; buttons "Place an order with an SL limit" · "Place without limit".
- 2026-11-05 · Layout 5d: tighter: the risk line follows the stamp, the bar grows from its centre as the words are halfway up, and Everyone → You is quicker (≈ 5 s in all).
- 2026-11-05 · Layouts 2 · Big number and 3 · Compact alert archived (out of the Layout setting; code kept in NudgeSheet, a stored value falls back to Layout 5). Phone app mode: two floating one-tap buttons on the order pad screens — 5A–5D (Layout 5 version) and L1 · L4 · L5 (layout) — in place of the app-wide V8 button.
- 2026-11-19 · Layout 5e · Fact first, 5f · One card, 5g · Compact (Case pill on Layout 5): 5a re-laid out after a design review — the fact leads, plain-word markers on a calmer scale, no prediction tooltip or looping glow; 5g has side-by-side buttons. Branch `Anshul_Risk_Meter`.
- 2026-11-19 · Layout 4 · Risk meter is the default, for every case: the fact as the heading, the consequence under it in secondary (no tag), secondary end labels. New **Meter** setting with five styles — 4a Needle, 4b Ticks, 4c Sweep, 4d Knob, 4e Levels — each with a blurred glow behind every level as the reading reaches it (`sheets/Meter.tsx`). Branch `Anshul_Risk_Meter`.
- 2026-11-19 · Meter 4f · Soft dial (from the user's reference) is the default: thick gradient ring with dots, ticks and a dark marker, centre disc with the multiple and the level word; the dial and the word change green → yellow → red as it fills, with the blurred glow per level. The meter replays when the case or state changes.
- 2026-11-19 · 4f Soft dial: softer background blur (wider, about half as strong), fewer and smaller grains that travel with the needle (passed grains fade to 35 %), and a glass needle standing on the centre disc.
- 2026-11-19 · 4f Soft dial: the background glow is back to its earlier strength but in a lighter colour (level colour + 50 % white); the needle is solid black (no glass); the glass effect moved onto the filled part of the ring.
- 2026-11-19 · 4f Soft dial: the blur is no longer cut at the left / top (mask and filter areas reach past the ring); one continuous green → yellow → red blend; the needle eases in and out, overshoots and swings back before it settles; the sub line in the disc is gone; on Layout 4 the fact heading now sits above the meter.
- 2026-11-19 · 4f Soft dial: until the needle starts, the ring is plain grey with no glow or bloom; the tint, colours and blurred glows all start with the needle.
- 2026-11-19 · 4f Soft dial: a grain trail rides just behind the needle (30 grains, densest, biggest and brightest at the needle, twinkling while it runs; it turns with the needle, bounce included); grains left on the green and yellow levels fade away once the needle moves on, the red level keeps a few; the level word swaps out-then-in (no overlap).
- 2026-11-19 · 4f Soft dial: the bigger grain cluster at the needle is gone — back to the earlier even grains, a little brighter (60 %) in a soft wedge that rides behind the needle; the grains on the green and yellow levels still fade away as the needle moves on.
- 2026-11-19 · Layout 4 · Risk meter copy: heading "You are overtrading", sub line "Your chances of losing money today are high"; under the meter, size-up reads "You are placing a bigger 2X order right now after a loss of ₹2,900" (was "Last time you did this, you lost ₹2,900"), other cases show their own fact there.
- 2026-11-19 · Layout 4 · Soft dial: the centre shows only the level word — Safe, Caution or High risk — no "2X". New **Risk level** setting (pill on the floating switcher): the reading stops in the green (Safe), yellow (Caution) or red (High risk) level, with that level's colours, glow, grains and word (Safe stays green; Caution blends green → yellow). The heading "You are overtrading" is heading-24 (was 20), centred with its sub line, no separator; 24 above and below the meter; the fact under it is body text (description-14) in secondary.
- 2026-11-19 · Layout 4 · Soft dial, two new versions (pills on the floating switcher): **Meter layout → Meter first** — the meter on top, then "You are overtrading" (heading-24) and one line under it, "You are placing a bigger 2X order right now and your chances of losing money today are high" (other cases: their fact + "Your chances of losing money today are high"); **Meter fill → Blended** — each part of the ring keeps the colour it was filled with, green → yellow → red, melting into each other over a soft band at each level edge (oklab, smoothstep), drawn as 90 slices under a radial shade; the default stays One colour (the whole ring changes colour).
- 2026-11-19 · Layout 4 meter-first line: "You are placing a bigger **2X** order right now, and your chances of losing money are high, as you have already made a loss of **₹2,900**" — the multiple and the loss in bold (weight 750, primary). The gap from it to the button dock's line stays 24 (the content's bottom space).
- 2026-11-19 · Meter **4g · Blocks** (from the user's reference): the half ring as 13 thick rounded blocks with gaps; up to the reading they light up one by one in their level's colour — green, yellow, red — the rest stay grey; the level word in the middle; follows the Risk level setting and works with both meter layouts (`sheets/BlockDial.tsx`).
- 2026-11-19 · Meter-first line shortened to fit two lines: "A **2X** bigger order right after a **₹2,900** loss raises your chances of losing more today" (bold multiple and loss kept).
- 2026-11-19 · Layout 4, third meter layout **Tag first**: a red secondary L3 Tag "Overtrading Detected" (md), 12 below it the heading "Your chances of losing money today are high" (heading-18), 24 to the meter, 24 to the fact as body text ("You are placing a bigger 2X order right now after a loss of ₹2,900").
- 2026-11-19 · 4g · Blocks in the soft dial's design language: each block has depth (deep inside, light outside) and glass (sheen, soft highlight); grey blocks have a soft gradient; a fast eased sweep (1.8 s, the needle's run with its overshoot and bounce) fills the blocks, resting on the last lit block's edge; a blurred glow per level as the fill reaches it, a bloom riding the tip in the current level's colour, and the word swapping Safe → Caution → High risk on the way.
- 2026-11-19 · Tag first: the tag has a red exclamation (Material "error") in L3 Tag's left icon slot; the heading is "Your chances of losing money are high" (one line).
- 2026-11-19 · Tag first: the tag's icon is a filled triangle exclamation (Material warning-fill) in the tag's red, 2 from "Overtrading Detected", 8 padding left and right (L3 gap: Medium Tag has 2 at the sides and 6 icon → text, no prop to change it). Soft dial level edges moved: green 0–33 %, yellow 33–60 %, red 60–100 %; High risk lands at ~85 % (2X), Caution at 47 %, Safe at 20 %.
- 2026-11-19 · Soft dial: the moved level edges and deeper needle apply to the **Blended** fill only (green 0–33 %, yellow 33–60 %, red 60–100 %; High risk at ~85 %); the **One colour** fill is back to 45 % / 65 % with High risk at ~75 % (2X).
- 2026-11-19 · 4g · Blocks: no glass (gradient only); a wave travels with the fill — the block under the fill's head lifts 7 outward and settles back as it moves on, following the same eased path with its overshoot and back-swing.
- 2026-11-19 · Meter layout **With multiple**: as Text first, with the multiple big in the dial (heading-36, "2X") above the level word — soft dial and blocks — and the fact "You are placing a bigger order right after a loss of ₹2,900" with the loss in red (content/accent-error, weight 650).
- 2026-11-19 · With multiple: the fact reads "You’re placing a bigger order right after a loss of ₹2,900". Soft dial and Blocks: the drawing area now reaches 16 past the ring on each side and 30 above / below (viewBox −16 −24 372 240, the svg pulled back by negative margins so the dial doesn't move) — mobile Safari clips SVG at the svg's box, which cut the blurred glows off in hard edges.
- 2026-11-19 · Size-up body text in every meter layout (Text first, Meter first, Tag first, With multiple): "You are placing a bigger order right after a loss" — no multiple, no amount (the bold 2X / ₹2,900 and the red loss are gone).
- 2026-11-19 · Meter + body text read as one block: the dial's box now ends at the ring's base line (soft dial and blocks, svg bottom margin −40/340) and the body text sits 16 under it (Text first, Tag first, With multiple); 24 from the sub line to the meter and 24 from the body text to the dock's line, as before.
- 2026-11-19 · Tag first: the dial shows the multiple too — "2X" above "High risk", as in With multiple (soft dial and blocks).
- 2026-11-19 · Layout 4: 32 from the last text (the body text, or Meter first's line) to the dock's line (was L3's 24).
- 2026-11-19 · Cut down to the risk meter: Layouts 1 and 5 (5a–5g), meters 4a–4e and their settings removed (Layout, Layout 1 header, Layout 5 versions, the app-mode quick switches); kept 4f Soft dial and 4g Blocks with Risk level, Meter layout and Meter fill. The dials' drawing area now reaches 70 above the dial (was 30), so the blurred glow isn't cut off on mobile; text drawn above the glow.
- 2026-11-20 · The dial's multiple ("2X") is removed from every version (Text first, Meter first, Tag first, With multiple; soft dial and blocks): the disc shows only the level word. With multiple now looks the same as Text first.
- 2026-11-20 · Meters **4h · Speedometer · 0–100** and **4i · Speedometer · Low–High** from Figma (6917:62779), with their own layout and motion (segments light up, the needle swings up and settles). Meter fill **Blended · 3 blocks**: the blended soft dial cut into three blocks where the colour changes, each block in its own colour.
- 2026-11-20 · 4h / 4i speedometer heading: "Your chances of losing money is high" (was "Your portfolio is at high risk"); medium / low for the other levels.
- 2026-11-20 · 4h / 4i speedometer: the fact under the heading in L3 Description/14 (was Label/12).
- 2026-11-20 · Meter fill **3 blocks · Low–High**: the 3-block soft dial with the speedometer's Low · Medium · High scale round it (the dial a little smaller to make room).
- 2026-11-20 · 3 blocks · Low–High: three equal blocks (Medium now over the middle of the yellow), the scale words at 14, and the disc's word reads Low risk → Medium risk → High risk as the needle moves.
- 2026-11-20 · 3 blocks · Low–High: no word in the dial; under it the speedometer's text — the risk Tag (High Risk), 16, "Your chances of losing money is high" (Heading/20), 8, the fact (Description/14) — and 24 to the dock's line; Meter layout doesn't apply (hidden in the switcher).
- 2026-11-20 · 3 blocks · Low–High: the risk Tag moves into the dial's disc (where the level word was) and fades in as the needle reaches the reading; the heading and fact follow the dial; Low · Medium · High at 16.
- 2026-11-20 · 3 blocks · Low–High: 24 from the dial to the heading (was 16).
- 2026-11-20 · Meter fill **One colour · Low–High**: the one-colour soft dial (the whole ring turns green → yellow → red) with the 3 blocks · Low–High design — equal thirds, Low · Medium · High round it, the High Risk tag in the disc fading in as the needle lands, the heading and fact 24 under it.
- 2026-11-20 · Blended · 3 blocks: three equal blocks (green, yellow, red a third each; low stops at ⅙, medium at ½), as in 3 blocks · Low–High.


## Source — this flow

### `src/flows/nudges/components/SheetSwitcher.module.css`

```css
/* PROTOTYPE ONLY: the floating Layout · Case · State pill over the open nudge sheet. Tokens only. */

/* Starts top-left, under the floating settings button and above the sheet; once dragged, left / top come from the
   component. */
.pill {
  position: absolute;
  z-index: var(--app-layer-fab);
  left: var(--l3-spacing-12);
  top: var(--l3-size-64);
  width: max-content;
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-04);
  max-width: calc(100% - var(--l3-spacing-24));
  padding: var(--l3-spacing-04);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-primary);
  box-shadow: var(--l3-shadow-elevation-high);
  touch-action: none;
}

.grip {
  flex: none;
  display: grid;
  place-items: center;
  width: var(--l3-size-32);
  height: var(--l3-size-32);
  padding: 0;
  border: 0;
  border-radius: var(--l3-radius-full);
  background: none;
  color: var(--l3-content-secondary);
  cursor: grab;
  touch-action: none;
}
.grip:active { cursor: grabbing; }

.chip {
  flex: 0 1 auto;
  min-width: 0;
  display: inline-flex;
  align-items: center;
  gap: var(--l3-spacing-02);
  height: var(--l3-size-32);
  padding: 0 var(--l3-spacing-04) 0 var(--l3-spacing-08);
  border: 0;
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-secondary);
  color: var(--l3-content-primary);
  font: var(--l3-text-label-12);
  white-space: nowrap;
  cursor: pointer;
}
.chip > svg { flex: none; transition: transform var(--l3-motion-duration-short) var(--l3-motion-easing-standard); }
.chip[data-open] { box-shadow: inset 0 0 0 1px var(--l3-border-dark); }
.chip[data-open] > svg { transform: rotate(180deg); }
/* The short pills keep their size; the case (the long one) gives way so the bar never runs past the screen. */
.value { min-width: 0; overflow: hidden; text-overflow: ellipsis; }
.chip:not([data-wide]) { flex: none; }
.chip[data-wide] { min-width: var(--l3-size-64); }

/* The dropdown: a floating card with the full names. */
.menu {
  position: absolute;
  z-index: var(--app-layer-fab);
  box-sizing: border-box;
  min-width: calc(var(--l3-size-96) * 2);
  max-width: calc(100% - var(--l3-spacing-16));
  overflow-y: auto;
  padding: var(--l3-spacing-04);
  border-radius: var(--l3-radius-16);
  background: var(--l3-surface-primary);
  box-shadow: var(--l3-shadow-elevation-high), inset 0 0 0 1px var(--l3-border-light);
  animation: drop var(--l3-motion-duration-short) var(--l3-motion-easing-decelerate) both;
}
.menuTitle {
  padding: var(--l3-spacing-08) var(--l3-spacing-12) var(--l3-spacing-04);
  color: var(--l3-content-secondary);
  font: var(--l3-text-label-12);
}
.item {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  width: 100%;
  min-height: var(--l3-size-40);
  padding: 0 var(--l3-spacing-12);
  border: 0;
  border-radius: var(--l3-radius-12);
  background: none;
  color: var(--l3-content-primary);
  font: var(--l3-text-label-14);
  text-align: left;
  cursor: pointer;
}
.item:hover { background: var(--l3-surface-secondary); }
.item[data-picked] { background: var(--l3-surface-secondary); }
.itemLabel { flex: 1; }
.item > svg { flex: none; color: var(--l3-content-accent-success-default); }

@keyframes drop { from { opacity: 0; transform: translateY(calc(var(--l3-spacing-04) * -1)); } }
@media (prefers-reduced-motion: reduce) { .menu, .chip > svg { animation: none; transition: none; } }
```

### `src/flows/nudges/components/SheetSwitcher.tsx`

```tsx
import { useEffect, useLayoutEffect, useRef, useState, type PointerEvent } from 'react'
import { createPortal } from 'react-dom'
import { Icon } from '@l3/components/Icon'
import { msCheck, msDragIndicator, msKeyboardArrowDown } from '@l3/icons/material'
import { setFlowOption, useFlowOption, usePreview } from '../../../shared/prototype'
import { t } from '../copy'
import { activeMeter, caseOption, meterFillOption, meterLayoutOption, meterLevelOption, meterOption, stateOption } from '../options'
import styles from './SheetSwitcher.module.css'

type Option = typeof caseOption | typeof stateOption | typeof meterOption | typeof meterLevelOption | typeof meterLayoutOption | typeof meterFillOption
type Spot = { x: number; y: number }

const SPOT_KEY = 'nudges-switcher-spot'
const readSpot = (): Spot | null => {
  try { return JSON.parse(localStorage.getItem(SPOT_KEY) ?? 'null') } catch { return null }
}

/**
 * PROTOTYPE ONLY — not part of the design. A small floating pill over the order pad and the open nudge sheet: Layout ·
 * (Layout 1: Chip / Icon + tag) · Case · State,
 * each a dropdown pill (its own menu, with the full names), so versions can be compared on a phone without opening the
 * settings. A value that doesn't fit its pill is cut with an ellipsis; the menu shows it whole. Drag it by the grip; it remembers where it was left (this browser).
 *
 * The sheet makes everything inside the screen inert, so the pill is drawn just outside the screen (in the layer
 * that holds it), over the sheet.
 */
/** `open`: show it (on a pad that has a nudge to fire). */
export function SheetSwitcher({ open }: { open: boolean }) {
  const { interactive } = usePreview()
  const anchor = useRef<HTMLSpanElement>(null)
  const [host, setHost] = useState<HTMLElement | null>(null)
  useLayoutEffect(() => {
    const screen = anchor.current?.closest('[data-part="page"]')?.parentElement
    setHost(screen?.parentElement ?? null)
  }, [])

  return <>
    <span ref={anchor} hidden />
    {interactive && open && host && createPortal(<Pill host={host} />, host)}
  </>
}

function Pill({ host }: { host: HTMLElement }) {
  const pill = useRef<HTMLDivElement>(null)
  const [spot, setSpot] = useState<Spot | null>(readSpot)
  const meter = activeMeter(useFlowOption('nudges', meterOption.id, meterOption.default))
  const fill = useFlowOption('nudges', meterFillOption.id, meterFillOption.default)
  const [menu, setMenu] = useState<{ option: Option; chip: HTMLElement } | null>(null)
  const grab = useRef<{ id: number; dx: number; dy: number } | null>(null)

  // Keep it inside the screen (a phone turned, or a spot saved on a bigger screen).
  const clamp = (x: number, y: number): Spot => {
    const box = pill.current?.getBoundingClientRect()
    const w = box?.width ?? 0
    const h = box?.height ?? 0
    return { x: Math.min(Math.max(0, x), host.clientWidth - w), y: Math.min(Math.max(0, y), host.clientHeight - h) }
  }

  const down = (e: PointerEvent<HTMLButtonElement>) => {
    const box = pill.current!.getBoundingClientRect()
    setMenu(null)
    grab.current = { id: e.pointerId, dx: e.clientX - box.left, dy: e.clientY - box.top }
    try { e.currentTarget.setPointerCapture(e.pointerId) } catch { /* the pointer is already gone */ }
  }
  const move = (e: PointerEvent<HTMLButtonElement>) => {
    const g = grab.current
    if (!g || g.id !== e.pointerId) return
    const origin = host.getBoundingClientRect()
    // The preview may scale the phone: convert screen pixels to the host's own.
    const scale = origin.width / host.clientWidth || 1
    setSpot(clamp((e.clientX - origin.left - g.dx) / scale, (e.clientY - origin.top - g.dy) / scale))
  }
  const up = (e: PointerEvent<HTMLButtonElement>) => {
    if (grab.current?.id !== e.pointerId) return
    grab.current = null
    try { if (spot) localStorage.setItem(SPOT_KEY, JSON.stringify(spot)) } catch { /* storage off */ }
  }

  return <div ref={pill} className={styles.pill} role="group" aria-label={t('switcher.label')}
    style={spot ? { left: spot.x, top: spot.y } : undefined}>
    <button type="button" className={styles.grip} aria-label={t('switcher.drag')}
      onPointerDown={down} onPointerMove={move} onPointerUp={up} onPointerCancel={up}>
      <Icon icon={msDragIndicator} size={20} />
    </button>
    <Pick option={meterOption} menu={menu} setMenu={setMenu} short={(_, label) => label.split(' · ')[0]} />
    <Pick option={meterLevelOption} menu={menu} setMenu={setMenu} short={(_, label) => label} />
    {!meter.startsWith('gauge') && !(meter === 'soft' && (fill === 'splitWords' || fill === 'wholeWords')) && <Pick option={meterLayoutOption} menu={menu} setMenu={setMenu} short={(_, label) => label} />}
    {meter === 'soft' && <Pick option={meterFillOption} menu={menu} setMenu={setMenu} short={(_, label) => label} />}
    <Pick option={caseOption} menu={menu} setMenu={setMenu} short={(_, label) => label} wide />
    <Pick option={stateOption} menu={menu} setMenu={setMenu} short={(v) => t('switcher.state', { state: v })} />
    {menu && <Menu option={menu.option} chip={menu.chip} host={host} onClose={() => setMenu(null)} />}
  </div>
}

type MenuState = { option: Option; chip: HTMLElement } | null

/** A dropdown pill: the short value (cut with an ellipsis when it doesn't fit) and a chevron; a tap opens the menu. */
function Pick({ option, short, wide, menu, setMenu }: {
  option: Option; short: (value: string, label: string) => string; wide?: boolean
  menu: MenuState; setMenu: (menu: MenuState) => void
}) {
  const value = useFlowOption('nudges', option.id, option.default)
  const label = option.values.find((o) => o.value === value)?.label ?? value
  const open = menu?.option.id === option.id
  return <button type="button" className={styles.chip} data-wide={wide ? '' : undefined} data-open={open ? '' : undefined}
    aria-haspopup="listbox" aria-expanded={open} aria-label={`${option.label}: ${label}`}
    onClick={(e) => setMenu(open ? null : { option, chip: e.currentTarget })}>
    <span className={styles.value}>{short(value, label)}</span>
    <Icon icon={msKeyboardArrowDown} size={16} />
  </button>
}

/** The open dropdown: every value by its full name, the picked one ticked. Under its pill (above it when there's no
 *  room), kept inside the screen; a tap outside or Escape closes it. */
function Menu({ option, chip, host, onClose }: { option: Option; chip: HTMLElement; host: HTMLElement; onClose: () => void }) {
  const box = useRef<HTMLDivElement>(null)
  const value = useFlowOption('nudges', option.id, option.default)
  const [spot, setSpot] = useState<{ left: number; top: number; maxHeight: number } | null>(null)

  useLayoutEffect(() => {
    const origin = host.getBoundingClientRect()
    const scale = origin.width / host.clientWidth || 1
    const at = chip.getBoundingClientRect()
    const gap = 8
    const width = box.current?.offsetWidth ?? 0
    const left = Math.min(Math.max(gap, (at.left - origin.left) / scale), host.clientWidth - width - gap)
    const below = (at.bottom - origin.top) / scale + gap
    const above = (at.top - origin.top) / scale - gap
    const roomBelow = host.clientHeight - below - gap
    const height = box.current?.scrollHeight ?? 0
    setSpot(height <= roomBelow || roomBelow >= above
      ? { left, top: below, maxHeight: roomBelow }
      : { left, top: Math.max(gap, above - height), maxHeight: above - gap })
  }, [chip, host])

  useEffect(() => {
    const away = (e: Event) => {
      const target = e.target as Node
      if (!box.current?.contains(target) && !chip.contains(target)) onClose()
    }
    const key = (e: KeyboardEvent) => { if (e.key === 'Escape') onClose() }
    document.addEventListener('pointerdown', away)
    document.addEventListener('keydown', key)
    return () => { document.removeEventListener('pointerdown', away); document.removeEventListener('keydown', key) }
  }, [chip, onClose])

  return createPortal(<div ref={box} className={styles.menu} role="listbox" aria-label={option.label}
    style={spot ?? { visibility: 'hidden' }}>
    <div className={styles.menuTitle}>{option.label}</div>
    {option.values.map((o) => {
      const picked = o.value === value
      return <button key={o.value} type="button" role="option" aria-selected={picked} className={styles.item}
        data-picked={picked ? '' : undefined}
        onClick={() => { setFlowOption('nudges', option.id, option.default, o.value); onClose() }}>
        <span className={styles.itemLabel}>{o.label}</span>
        {picked && <Icon icon={msCheck} size={16} />}
      </button>
    })}
  </div>, host)
}
```

### `src/flows/nudges/copy.json`

```json
{
  "order.back": "Back",
  "order.switch": "Change contract",
  "order.product": "Product",
  "order.delivery": "Delivery",
  "order.intraday": "Intraday",
  "order.type": "Order type",
  "order.market": "Market",
  "order.limit": "Limit",
  "order.sl": "SL",
  "order.validity": "Validity",
  "order.quantity": "Quantity",
  "order.lots": "Lots",
  "order.unitSwitch": "Switch between quantity and lots",
  "order.lotHint": "({lot} = 1 Lot)",
  "order.limitPrice": "Limit",
  "order.trigger": "Trigger Price",
  "order.offset": "Distance from the market price",
  "order.tpsl": "Set TPSL",
  "order.margin": "Margin:",
  "order.leverage": "Leverage:",
  "order.autoClose": "This position will auto close by {time}",
  "order.available": "Avail.",
  "order.required": "Req",
  "order.buy": "Buy",
  "order.sell": "Sell",
  "order.placed": "{side} order placed · {name} {contract}",
  "silent.tpsl": "TP/SL",
  "silent.tpslOn": "Target +{target}% · Stop-loss −{stop}%",
  "silent.tpslOff": "Off",
  "silent.tpslEdit": "Edit TP/SL",
  "silent.tpslToggle": "TP/SL on this order",
  "silent.prefilled": "Pre-filled",
  "silent.limitLeft": "{left} left of today's loss limit",
  "silent.limitAdd": "Set a loss limit for today",
  "silent.limitBar": "Loss used today",
  "silent.limitNear": "Close to today's loss limit",
  "nudge.setPnl": "Set a loss limit for today",
  "nudge.storyPrimary": "Place an order with an SL limit",
  "nudge.storyNotNow": "Place without limit",
  "nudge.setTpsl": "Add TP/SL to this order",
  "nudge.notNow": "Not now, place order",
  "pnl.title": "Today's loss limit",
  "pnl.description": "When today's loss reaches this amount, new F&O orders pause until tomorrow. Open positions stay as they are.",
  "pnl.field": "Limit for today",
  "pnl.presets": "Quick amounts",
  "pnl.helper": "You can change or remove it any time today.",
  "pnl.save": "Set limit",
  "pnl.saved": "Loss limit set · {amount} for today",
  "tpsl.title": "TP/SL for this order",
  "tpsl.description": "Exits on its own when the price reaches your target or your stop-loss.",
  "tpsl.target": "Target profit",
  "tpsl.stop": "Stop-loss",
  "tpsl.atPrice": "at {price}",
  "tpsl.save": "Save TP/SL",
  "tpsl.saved": "TP/SL added to this order",
  "option.case": "Case",
  "option.state": "Data state",
  "option.stateA": "A · own loss",
  "option.stateB": "B · own, no loss",
  "option.stateC": "C · thin history",
  "option.everyBuy": "Show on every Buy",
  "option.everyBuyOff": "Off",
  "option.everyBuyOn": "On",
  "option.vibrate": "Vibrate when it opens",
  "switcher.label": "Prototype: switch the nudge",
  "switcher.drag": "Drag to move",
  "switcher.layout": "Layout {n}",
  "switcher.state": "State {state}",
  "option.actions": "Buttons",
  "option.actionsTwo": "Both guardrails",
  "option.actionsOne": "One guardrail",
  "nudge.tag": "Overtrading Detected",
  "gauge.low": "Low",
  "gauge.medium": "Medium",
  "gauge.high": "High",
  "gauge.tag.danger": "High Risk",
  "gauge.tag.caution": "Medium Risk",
  "gauge.tag.safe": "Low Risk",
  "gauge.heading.danger": "Your chances of losing money is high",
  "gauge.heading.caution": "Your chances of losing money is medium",
  "gauge.heading.safe": "Your chances of losing money is low",
  "gauge.sizeUp": "You are placing a bigger order right after loss",
  "nudge.lastLoss": "Last time you did this, you lost {amount}",
  "nudge.others": "{count} in 10 traders who did this lost more by the end of the day",
  "nudge.othersOtherDays": "{count} in 10 traders who did this lost more than on other days",
  "nudge.notNowPositions": "Not now",
  "nudge.barsLabel": "{now}: {ratio}× {base}",
  "nudge.usual": "Usual",
  "nudge.vsUsual": "vs your usual",
  "nudge.highRisk": "High risk",
  "nudge.levelLow": "Low risk",
  "nudge.levelMedium": "Medium risk",
  "nudge.levelSafe": "Safe",
  "nudge.levelMid": "Caution",
  "nudge.vsUsualPlain": "Compared with how you usually trade",
  "nudge.zoneSafe": "Safe",
  "nudge.zoneDanger": "Danger",
  "nudge.zoneLow": "Low risk zone",
  "nudge.zoneHigh": "High risk zone",
  "nudge.zoneHeadline": "Overtrading, you just entered the high risk zone",
  "nudge.overtrading": "You're overtrading",
  "nudge.overtradingRisk": "Your chances of losing money are high",
  "nudge.meterHeading": "You are overtrading",
  "nudge.meterSub": "Your chances of losing money today are high",
  "nudge.meterRiskHeading": "Your chances of losing money are high",
  "nudge.sizeUpNow": "You are placing a bigger {ratio}X order right now after a loss of {amount}",
  "nudge.sizeUpNowPlain": "You are placing a bigger order right now after a loss of {amount}",
  "nudge.biggerAfterLoss": "You are placing a bigger order right after a loss",
  "nudge.meterFirstSub": "A {ratio} bigger order right after a {amount} loss raises your chances of losing more today",
  "nudge.meterFirstSubPlain": "A bigger order right after a {amount} loss raises your chances of losing more today",
  "nudge.meterFirstSubOther": "{fact}. Your chances of losing money today are high",
  "nudge.numberFact": "You’re placing a bigger order right after a loss of {amount}",
  "nudge.everyone": "Everyone",
  "nudge.you": "You",
  "nudge.safeZone": "Safe zone",
  "nudge.dangerZone": "Danger zone",
  "nudge.safeZoneNote": "trading to plan",
  "nudge.dangerZoneNote": "chasing losses",
  "nudge.lossChance": "High chance of losing money",
  "nudge.factWhy": "You're in a loss today. This is based on your own trades.",
  "nudge.riskLow": "Lower risk",
  "nudge.riskHigh": "Higher risk",
  "nudge.scaleLabel": "{now} is {ratio}X {base}, at the higher-risk end",
  "nudge.setPnlShort": "Set loss limit",
  "nudge.placeOrder": "Place order",
  "nudge.meterLabel": "Risk meter: you are in the danger zone",
  "nudge.meterLabelSafe": "Risk meter: you are in the safe zone",
  "nudge.meterLabelCaution": "Risk meter: you are in the caution zone",
  "nudge.meterLabelRatio": "Risk meter: you are in the danger zone, {ratio}X your usual",
  "case.generic.name": "Generic",
  "case.generic.headline": "Down ₹2,500 today and still trading",
  "case.sizeUp.name": "1 · Size-up after a loss",
  "case.sizeUp.headline": "This order is 2X the one you just lost",
  "case.sizeUp.base": "Trade you lost",
  "case.sizeUp.now": "This order",
  "case.sizeUp.unit": "bigger than the trade you just lost",
  "case.orders.name": "2 · Orders in the session",
  "case.orders.headline": "3X more orders than usual today",
  "case.orders.raw": "14 orders today",
  "case.orders.base": "Your usual day",
  "case.orders.now": "Today",
  "case.orders.unit": "more orders than usual today",
  "case.flips.name": "3 · CE ↔ PE flips",
  "case.flips.headline": "Switched calls ↔ puts 3X more than usual",
  "case.flips.raw": "Switched calls ↔ puts 6 times today",
  "case.flips.base": "Your usual day",
  "case.flips.now": "Today",
  "case.flips.unit": "more call ↔ put switches than usual",
  "case.burst.name": "4 · Orders in 15 min",
  "case.burst.headline": "4X more orders than usual in 15 min",
  "case.burst.raw": "9 orders in the last 15 min",
  "case.burst.base": "Usual 15 min",
  "case.burst.now": "Last 15 min",
  "case.burst.unit": "more orders than usual in 15 min",
  "case.money15.name": "5 · Money in 15 min",
  "case.money15.headline": "3X more money than usual in 15 min",
  "case.money15.raw": "₹48,000 put in, in 15 min",
  "case.money15.base": "Usual 15 min",
  "case.money15.now": "Last 15 min",
  "case.money15.unit": "more money than usual in 15 min",
  "case.bigOrder.name": "6 · Big order vs money",
  "case.bigOrder.headline": "This order is 4X your usual size",
  "case.bigOrder.raw": "This order uses 60% of your money",
  "case.bigOrder.base": "Your usual order",
  "case.bigOrder.now": "This order",
  "case.bigOrder.unit": "your usual order size",
  "case.pace.name": "7 · Time between orders",
  "case.pace.headline": "Ordering 6X faster than usual",
  "case.pace.raw": "40 seconds between orders",
  "case.pace.base": "Your usual pace",
  "case.pace.now": "Now",
  "case.pace.unit": "faster than your usual pace",
  "case.strike.name": "8 · Far strike",
  "case.strike.headline": "Strike 10X farther than usual",
  "case.strike.raw": "Strike 1,200 points from NIFTY",
  "case.strike.base": "Your usual strike",
  "case.strike.now": "This strike",
  "case.strike.unit": "farther than your usual strike",
  "case.avgDown.name": "9 · Averaging down",
  "case.avgDown.headline": "Bought more 3 times while it fell",
  "case.rareSession.name": "10 · Unusual time",
  "case.rareSession.headline": "Trading at a time you rarely trade",
  "case.expiry.name": "Moment · Expiry, recovering",
  "case.expiry.headline": "3X more trades than usual, on expiry",
  "case.expiry.raw": "12 trades today, on expiry",
  "case.expiry.base": "Your usual day",
  "case.expiry.now": "Today",
  "case.expiry.unit": "more trades than usual, on expiry",
  "case.deposit.name": "Moment · Bigger deposit",
  "case.deposit.headline": "Added 5X more money than usual",
  "case.deposit.raw": "Added ₹20,000 today",
  "case.deposit.base": "Your usual top-up",
  "case.deposit.now": "Today",
  "case.deposit.unit": "more money added than usual",
  "case.givingBack.name": "Moment · Giving back profit",
  "case.givingBack.headline": "Gave back 79% of today's profit",
  "case.fomo.name": "Moment · Missed entry",
  "case.fomo.headline": "Buying after it already rose 38%",
  "case.returnBreak.name": "Moment · Back after a break",
  "case.returnBreak.headline": "3X your usual size, after a break",
  "case.returnBreak.raw": "First trade in 23 days, and a big one",
  "case.returnBreak.base": "Your usual order",
  "case.returnBreak.now": "This order",
  "case.returnBreak.unit": "your usual size, after a break",
  "case.afterWin.name": "Moment · After a winning day",
  "case.afterWin.headline": "2X your usual size, after a win",
  "case.afterWin.raw": "Big order the day after a ₹6,200 win",
  "case.afterWin.base": "Your usual order",
  "case.afterWin.now": "This order",
  "case.afterWin.unit": "your usual size, after a win",
  "option.layout": "Layout",
  "option.layoutBars": "1 · Centred + bars",
  "option.layoutNumber": "2 · Big number",
  "option.layoutCompact": "3 · Compact alert",
  "option.layoutGauge": "4 · Risk meter",
  "option.zoneRisk": "5a · High risk",
  "option.zoneEveryone": "5b · Everyone vs you",
  "option.zoneExplain": "5c · Why first",
  "option.zoneStory": "5d · Story",
  "option.zoneFact": "5e · Fact first",
  "option.zoneCard": "5f · One card",
  "option.zoneCompact": "5g · Compact",
  "option.meter": "Meter",
  "option.meterNeedle": "4a · Needle",
  "option.meterTicks": "4b · Ticks",
  "option.meterSweep": "4c · Sweep",
  "option.meterKnob": "4d · Knob",
  "option.meterLevels": "4e · Levels",
  "option.meterSoft": "4f · Soft dial",
  "option.meterBlocks": "4g · Blocks",
  "option.meterGauge": "4h · Speedometer · 0–100",
  "option.meterGaugeWords": "4i · Speedometer · Low–High",
  "option.meterLevel": "Risk level",
  "option.levelDanger": "High risk",
  "option.levelCaution": "Caution",
  "option.levelSafe": "Safe",
  "option.meterLayout": "Meter layout",
  "option.meterLayoutText": "Text first",
  "option.meterLayoutMeter": "Meter first",
  "option.meterLayoutTag": "Tag first",
  "option.meterLayoutNumber": "With multiple",
  "option.meterFill": "Meter fill",
  "option.meterFillWhole": "One colour",
  "option.meterFillBlend": "Blended",
  "option.meterFillSplit": "Blended · 3 blocks",
  "option.meterFillSplitWords": "3 blocks · Low–High",
  "option.meterFillWholeWords": "One colour · Low–High",
  "option.layoutZone": "5 · Risk bar",
  "option.badge": "Layout 1 header",
  "option.badgeChip": "Chip (✋ in the tag)",
  "option.badgeTag": "Icon + tag",
  "switcher.badgeChip": "Chip",
  "switcher.badgeTag": "Tag"
}
```

### `src/flows/nudges/copy.ts`

```ts
import { makeCopy } from '../../shared/copy'
import copy from './copy.json'

/** Every user-facing string of this flow is in copy.json: t('screen.title'), t('key', { count }). */
export const t = makeCopy('nudges', copy)
```

### `src/flows/nudges/data.ts`

```ts
/**
 * Overtrading nudges (product team draft, 2026-10-30 — playground, nothing built in the real app).
 *
 * Every nudge is ONE sheet; a case only fills its slots. The words live in copy.json under `case.<id>.*`; this file
 * says which cases exist, which guardrail each one leads with, and the sample account values. Every number in the
 * copy is an example: the real values are computed per trader (open item 1 in the brief).
 */

/** The two guardrails a nudge can offer. */
export type Guardrail = 'pnl' | 'tpsl'

/**
 * How much of the trader's own history we can use for this case:
 * - A: enough history, and the last time he did this he lost → the consequence is his own loss.
 * - B: enough history, the last time didn't lose → the consequence is what happened to other traders.
 * - C: thin history (no "usual" yet) → the headline is today's raw fact, no bars; the consequence as in B.
 * (D — the behaviour can't be scored — never shows its own sheet: the generic sheet fires instead.)
 */
export type DataState = 'A' | 'B' | 'C'

export type NudgeCaseId =
  | 'generic'
  | 'sizeUp' | 'orders' | 'flips' | 'burst' | 'money15' | 'bigOrder' | 'pace' | 'strike' | 'avgDown' | 'rareSession'
  | 'expiry' | 'deposit' | 'givingBack' | 'fomo' | 'returnBreak' | 'afterWin'

export type NudgeCase = {
  id: NudgeCaseId
  /** generic: fires with the revenge flag when nothing stands out · revenge: one of the 10 inputs of the v2.5 score ·
   *  moment: a journey moment that fires without the revenge flag. */
  kind: 'generic' | 'revenge' | 'moment'
  /** Where it fires: on the order pad (as he taps Buy / Sell) or on Positions (no order in hand). */
  where: 'order' | 'positions'
  /** The guardrail on the main button. */
  primary: Guardrail
  /** Today vs his usual (or vs the trade he lost), drawn as two bars. None = the fact isn't a multiple. */
  ratio?: number
  /** Traders who did this and lost more by the end of the day (%). None = not measured → no group line. */
  othersLostMore?: number
  /** State A: what he lost the last time he did this (₹). None = the case has no own-history version. */
  lastLoss?: number
  /** The group line compares with the same traders' other days, not the end of the day. */
  othersVs?: 'otherDays'
}

/** The 17 cases, values from the product team's compare file (2026-11-01). */
export const nudgeCases: NudgeCase[] = [
  { id: 'generic', kind: 'generic', where: 'positions', primary: 'pnl', othersLostMore: 67 },
  { id: 'sizeUp', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 2, othersLostMore: 69, lastLoss: 2900 },
  { id: 'orders', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 3, othersLostMore: 68, lastLoss: 3600 },
  { id: 'flips', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 3, othersLostMore: 69, lastLoss: 3400 },
  { id: 'burst', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 4, othersLostMore: 68, lastLoss: 4800 },
  { id: 'money15', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 3, othersLostMore: 68, lastLoss: 6100 },
  { id: 'bigOrder', kind: 'revenge', where: 'order', primary: 'tpsl', ratio: 4, othersLostMore: 66, lastLoss: 5200 },
  { id: 'pace', kind: 'revenge', where: 'order', primary: 'pnl', ratio: 6, othersLostMore: 70, lastLoss: 2500 },
  { id: 'strike', kind: 'revenge', where: 'order', primary: 'tpsl', ratio: 10, othersLostMore: 72, lastLoss: 1700 },
  { id: 'avgDown', kind: 'revenge', where: 'order', primary: 'tpsl', othersLostMore: 66, lastLoss: 3900 },
  { id: 'rareSession', kind: 'revenge', where: 'order', primary: 'pnl', othersLostMore: 67, lastLoss: 2200 },
  { id: 'expiry', kind: 'moment', where: 'positions', primary: 'pnl', ratio: 3, othersLostMore: 66, lastLoss: 6800 },
  { id: 'deposit', kind: 'moment', where: 'order', primary: 'pnl', ratio: 5, lastLoss: 4100 },
  { id: 'givingBack', kind: 'moment', where: 'positions', primary: 'pnl', lastLoss: 2300 },
  { id: 'fomo', kind: 'moment', where: 'order', primary: 'tpsl', lastLoss: 1800 },
  { id: 'returnBreak', kind: 'moment', where: 'order', primary: 'pnl', ratio: 3, lastLoss: 9800 },
  { id: 'afterWin', kind: 'moment', where: 'order', primary: 'tpsl', ratio: 2, othersLostMore: 60, lastLoss: 2700, othersVs: 'otherDays' },
]

export const caseById = new Map(nudgeCases.map((c) => [c.id, c]))

/** The order being placed when the nudge shows — the F&O commodity options order pad's sample (fno/order-pad/data.ts). */
export type OrderSide = 'buy' | 'sell'
export type OrderProduct = 'delivery' | 'intraday'
export type OrderType = 'market' | 'limit' | 'sl'
export const orderContract = {
  name: 'Gold',
  contract: '5 OCT Fut',
  exchange: 'MCX',
  ltp: 153580,
  change: 2.62,
  /** Units per lot: the quantity steps by one lot. */
  lotSize: 100,
}
export const sellMargin = { margin: '7.00%', leverage: '14.3X' }
export const funds = { available: 1105002, required: 1075060 }
/** Intraday positions are squared off automatically at this time (MCX evening session). */
export const intradayCloseTime = '11:30 PM'
export const validities = ['Day', 'IOC']

/** Silent help: the TP/SL the order pad pre-fills (% of the entry price). */
export const prefilledTpsl = { targetPct: 20, stopPct: 10 }

/** P&L limit: quick amounts and the sample day (lost so far today). */
export const limitPresets = [10000, 25000, 50000]
export const defaultLimit = 25000
export const lossToday = 0
/** "Near the limit" sample: ₹21,000 of the ₹25,000 limit used. */
export const lossNearLimit = 21000
```

### `src/flows/nudges/index.tsx`

```tsx
import type { Flow, Go } from '../types'
import { t } from './copy'
import { lossNearLimit } from './data'
import { nudgeOptions } from './options'
import { OrderPad } from './screens/OrderPad'

/** Back to wherever the order pad was opened from; the Gold MINI asset page (fno-21) when opened directly. */
const back = (go: Go) => () => { if (window.history.length > 1) window.history.back(); else go('fno-21') }

/**
 * PLAYGROUND — overtrading nudges (product team draft, 2026-10-30). Not a Figma flow: built from the brief in
 * playground/overtrading-nudges/README.md. Each screen is the order pad in one state; within a
 * screen, sheet → sheet changes swap over the same page.
 */
export const nudges: Flow = {
  id: 'nudges', title: 'Overtrading nudges (playground)',
  description: 'Journey-aware P&L limit and TP/SL prompts: silent help on the order pad, and one nudge sheet when the revenge flag and the journey engine agree.',
  figmaFile: '',
  options: nudgeOptions,
  screens: [
    { id: 'nudges-01', name: 'Order pad · silent help', figma: '8671:18433', note: 'The default — no interruption. The F&O commodity order pad (fno-25) as it is, plus silent help: “Set TPSL” becomes a TP/SL row, pre-filled and on (Target +20% · Stop-loss −10%); “Set a loss limit for today” is the first line of the dock. Buy fires the nudge picked in the settings (Case, Data state), once: the shared cap is 1 per session.', render: (go) => <OrderPad nudge="options" onBack={back(go)} /> },
    { id: 'nudges-02', name: 'Nudge · from settings', figma: '', note: 'The one nudge sheet — the risk meter — filled by the case and data state picked in the settings. Pills on the floating switcher: Meter (4f Soft dial · 4g Blocks), Risk level, Meter layout, Meter fill, Case, State.', render: (go) => <OrderPad nudge="options" initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-03', name: 'Nudge · generic', figma: '', note: 'Fires on Positions with the revenge flag when no single behaviour stands out, or the trader is too new to compare (state D). The meter and its own fact under it. Dismiss says just “Not now” (no order in hand).', render: (go) => <OrderPad nudge={{ caseId: 'generic', state: 'B' }} initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-04', name: 'Nudge · size-up · A (own loss)', figma: '', note: 'Size-up after a loss, own history and last loss (A): the meter at high risk, “You are placing a bigger order right after a loss”.', render: (go) => <OrderPad nudge={{ caseId: 'sizeUp', state: 'A' }} initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-05', name: 'Nudge · orders today · B', figma: '', note: 'Orders today, own history (B): the meter, and the fact “3X more orders than usual today” under it.', render: (go) => <OrderPad nudge={{ caseId: 'orders', state: 'B' }} initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-06', name: 'Nudge · orders today · C (thin history)', figma: '', note: 'Too few past sessions for “your usual”: the fact under the meter is today’s raw fact (“14 orders today”), no multiple.', render: (go) => <OrderPad nudge={{ caseId: 'orders', state: 'C' }} initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-07', name: 'Nudge · averaging down (TP/SL first)', figma: '', note: 'Averaging down: the main button is TP/SL; the fact under the meter is the case\'s own (no multiple).', render: (go) => <OrderPad nudge={{ caseId: 'avgDown', state: 'B' }} initialSheet="nudge" onBack={back(go)} /> },
    { id: 'nudges-08', name: 'Set today’s loss limit', figma: '', note: 'From the nudge’s “Set a loss limit for today” or the order pad dock. Pre-filled ₹25,000, quick amounts as pill tabs. Set limit → back to the pad with the limit shown.', render: (go) => <OrderPad nudge={{ caseId: 'sizeUp', state: 'A' }} initialSheet="pnl" onBack={back(go)} /> },
    { id: 'nudges-09', name: 'TP/SL for this order', figma: '', note: 'From the nudge’s “Add TP/SL to this order” or the TP/SL row. Target and stop-loss in %, the exit price under each.', render: (go) => <OrderPad nudge={{ caseId: 'avgDown', state: 'B' }} initialSheet="tpsl" onBack={back(go)} /> },
    { id: 'nudges-10', name: 'Order pad · limit set', figma: '', note: 'Silent help once a limit exists: “₹25,000 left of today’s loss limit” with a bar in the dock (tap to change it).', render: (go) => <OrderPad limit={25000} onBack={back(go)} /> },
    { id: 'nudges-11', name: 'Order pad · close to the limit', figma: '', note: '₹21,000 of ₹25,000 used: the bar and the line turn warning. Still no interruption.', render: (go) => <OrderPad limit={25000} loss={lossNearLimit} onBack={back(go)} /> },
  ],
}
```

### `src/flows/nudges/options.ts`

```ts
import { t } from './copy'
import { nudgeCases } from './data'

type Key = Parameters<typeof t>[0]
const onOff = [{ value: 'off', label: t('option.everyBuyOff') }, { value: 'on', label: t('option.everyBuyOn') }]

/**
 * PROTOTYPE ONLY: the flow's settings (preview panel / phone settings sheet) and, for the ones that change the sheet,
 * the floating switcher over the open nudge (components/SheetSwitcher). A real app ships one version.
 */
// Size-up after a loss is the default and heads the list.
export const caseOption = { id: 'case', group: 'Nudge', label: t('option.case'), picker: 'dropdown' as const, values: [...nudgeCases].sort((a, b) => Number(b.id === 'sizeUp') - Number(a.id === 'sizeUp')).map((c) => ({ value: c.id, label: t(`case.${c.id}.name` as Key) })), default: 'sizeUp' }
export const stateOption = { id: 'state', group: 'Nudge', label: t('option.state'), picker: 'dropdown' as const, values: [{ value: 'A', label: t('option.stateA') }, { value: 'B', label: t('option.stateB') }, { value: 'C', label: t('option.stateC') }], default: 'A' }
/** The meter (19 Nov, cut down to the two in use): 4f Soft dial (default) or 4g Blocks; 20 Nov: 4h / 4i the Figma
 *  speedometer (0–100 or Low–High scale, its own layout). A stored older style falls back to the default (activeMeter). */
export const meterOption = { id: 'meter', group: 'Nudge', label: t('option.meter'), picker: 'dropdown' as const, values: [{ value: 'soft', label: t('option.meterSoft') }, { value: 'blocks', label: t('option.meterBlocks') }, { value: 'gauge', label: t('option.meterGauge') }, { value: 'gaugeWords', label: t('option.meterGaugeWords') }], default: 'soft' }
export const activeMeter = (value: string) => meterOption.values.some((v) => v.value === value) ? value : meterOption.default

/** Where the reading stops (19 Nov) — the meter's three versions. */
export const meterLevelOption = { id: 'meterLevel', group: 'Nudge', label: t('option.meterLevel'), picker: 'dropdown' as const, values: [{ value: 'danger', label: t('option.levelDanger') }, { value: 'caution', label: t('option.levelCaution') }, { value: 'safe', label: t('option.levelSafe') }], default: 'danger' }

/** Where the text sits around the meter (19 Nov): Text first · Meter first · Tag first · With multiple. */
export const meterLayoutOption = { id: 'meterLayout', group: 'Nudge', label: t('option.meterLayout'), picker: 'dropdown' as const, values: [{ value: 'text', label: t('option.meterLayoutText') }, { value: 'meter', label: t('option.meterLayoutMeter') }, { value: 'tag', label: t('option.meterLayoutTag') }, { value: 'number', label: t('option.meterLayoutNumber') }], default: 'text' }
/** Soft dial only (19 Nov): the whole ring changes colour as the needle moves, or each part keeps the colour
 *  it was filled with, blended (green → yellow → red); or (20 Nov) blended and cut into three blocks where the colour
 *  changes; and that with Low · Medium · High round it, as on the speedometer; and (20 Nov) the one-colour ring with the same scale and tag. */
export const meterFillOption = { id: 'meterFill', group: 'Nudge', label: t('option.meterFill'), picker: 'dropdown' as const, values: [{ value: 'whole', label: t('option.meterFillWhole') }, { value: 'blend', label: t('option.meterFillBlend') }, { value: 'split', label: t('option.meterFillSplit') }, { value: 'splitWords', label: t('option.meterFillSplitWords') }, { value: 'wholeWords', label: t('option.meterFillWholeWords') }], default: 'whole' }

export const nudgeOptions = [
  caseOption,
  stateOption,
  meterOption,
  meterLevelOption,
  meterLayoutOption,
  meterFillOption,
  { id: 'everyBuy', group: 'Nudge', label: t('option.everyBuy'), picker: 'toggle' as const, values: onOff, default: 'on' },
  { id: 'vibrate', group: 'Nudge', label: t('option.vibrate'), picker: 'toggle' as const, values: onOff, default: 'on' },
  { id: 'actions', group: 'Nudge', label: t('option.actions'), values: [{ value: 'one', label: t('option.actionsOne') }, { value: 'two', label: t('option.actionsTwo') }], default: 'one' },
]
```

### `src/flows/nudges/screens/OrderPad.module.css`

```css
/* The F&O commodity options order pad (fno/order-pad, Figma 8671:18433) + the nudge's silent help, on L3 tokens. */
.contract { display: flex; flex-direction: column; align-items: flex-start; gap: var(--l3-spacing-02); color: var(--l3-content-primary); }
.contractName { display: inline-flex; align-items: center; gap: var(--l3-spacing-04); font: var(--l3-text-heading-14); }
.expiry { margin-left: var(--l3-spacing-04); font: var(--l3-text-label-12); color: var(--l3-content-tertiary); }
.contractPrice { display: inline-flex; align-items: center; gap: var(--l3-spacing-08); font: var(--l3-text-label-12); }
.exchange { font: var(--l3-text-label-12); color: var(--l3-content-primary); }

.body { padding: var(--l3-spacing-16); }
.validity.validity { min-height: var(--l3-size-32); border: 1px solid var(--l3-border-dark); border-radius: var(--l3-radius-full); padding-inline: var(--l3-spacing-12); }

/* Margin · Leverage (Sell): a grey strip. */
.margin { display: flex; justify-content: space-between; padding: var(--l3-spacing-12); border-radius: var(--l3-radius-12); background: var(--l3-surface-secondary); font: var(--l3-text-label-12); }
.margin strong, .funds strong { font: var(--l3-text-heading-12); color: var(--l3-content-primary); }
.muted { color: var(--l3-content-tertiary); }

/* A field row: label, value / hint, compact stepper at the right — bordered, 12 radius. */
.field { display: flex; align-items: center; gap: var(--l3-spacing-08); min-height: var(--l3-size-40); padding: var(--l3-spacing-04) var(--l3-spacing-04) var(--l3-spacing-04) var(--l3-spacing-12); border: 1px solid var(--l3-border-light); border-radius: var(--l3-radius-08); }
.fieldLabel { flex: 1; font: var(--l3-text-label-12); color: var(--l3-content-secondary); }
.fieldValue { font: var(--l3-text-heading-12); }
.unit { display: inline-flex; align-items: center; gap: var(--l3-spacing-02); padding: 0; border: 0; background: none; color: var(--l3-content-primary); font: var(--l3-text-label-12); cursor: pointer; }
.hint { flex: 1; font: var(--l3-text-label-12); color: var(--l3-content-tertiary); }
/* Steppers are the L3 Stepper Small as it is (dark + / −) — the design system's style wins over Figma's grey / blue. */

.tpslRow { display: flex; align-items: center; gap: var(--l3-spacing-12); padding: var(--l3-spacing-12); border: 1px solid var(--l3-border-light); border-radius: var(--l3-radius-08); }
.tpslText { flex: 1; display: flex; flex-direction: column; align-items: flex-start; gap: var(--l3-spacing-04); padding: 0; border: 0; background: none; color: var(--l3-content-primary); text-align: left; cursor: pointer; }
.tpslHead { display: inline-flex; align-items: center; gap: var(--l3-spacing-08); font: var(--l3-text-heading-12); }
.tpslValue { font: var(--l3-text-label-12); color: var(--l3-content-secondary); }

/* Dock: optional auto-close note (intraday), then Avail. · Req and the action. */
.dock { background: var(--l3-surface-primary); }
.autoClose { margin: 0; padding: var(--l3-spacing-12) var(--l3-spacing-16); font: var(--l3-text-label-12); color: var(--l3-content-tertiary); text-align: center; }
.dockBody { display: flex; flex-direction: column; gap: var(--l3-spacing-12); padding: var(--l3-spacing-12) var(--l3-spacing-16) var(--l3-spacing-16); border-top: 1px solid var(--l3-border-light); }
.funds { display: flex; justify-content: space-between; font: var(--l3-text-label-12); }


/* Silent help · loss limit, first in the dock: what's left of today's limit (bar), or the link to set one. */
.limit, .limitAdd { display: flex; width: 100%; padding: 0; border: 0; background: none; cursor: pointer; text-align: left; }
.limit { flex-direction: column; gap: var(--l3-spacing-08); }
.limitText, .limitAdd { display: inline-flex; align-items: center; gap: var(--l3-spacing-04); font: var(--l3-text-label-12); color: var(--l3-content-secondary); }
.limitText[data-tone='warning'] { color: var(--l3-content-accent-warning-default); }
.limitAdd { min-height: var(--l3-size-32); color: var(--l3-content-accent-discover-default); }
```

### `src/flows/nudges/screens/OrderPad.tsx`

```tsx
import { useState } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Aerobar } from '@l3/components/Aerobar'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { PriceChange } from '@l3/components/PriceChange'
import { ProgressBar } from '@l3/components/ProgressBar'
import { Select } from '@l3/components/Select'
import { Stack } from '@l3/components/Stack'
import { Stepper } from '@l3/components/Stepper'
import { Switch } from '@l3/components/Switch'
import { Tabs } from '@l3/components/Tabs'
import { Tag } from '@l3/components/Tag'
import { lmSwitchArrowVertical } from '@l3/icons/lemonnade'
import { msChevronRight, msShield } from '@l3/icons/material'
import { inr } from '../../../shared/data/format'
import { SHOWCASE, useFlowOption } from '../../../shared/prototype'
import type { RiskLevel } from '../sheets/SoftDial'
import { Screen } from '../../../shared/ui/Screen'
import { t } from '../copy'
import { activeMeter } from '../options'
import { caseById, defaultLimit, funds, intradayCloseTime, lossToday, orderContract, prefilledTpsl, sellMargin, validities, type DataState, type NudgeCaseId, type OrderProduct, type OrderSide, type OrderType } from '../data'
import { LossLimitSheet, TpslSheet } from '../sheets/GuardrailSheets'
import { NudgeSheet, type MeterLayout, type MeterStyle } from '../sheets/NudgeSheet'
import { SheetSwitcher } from '../components/SheetSwitcher'
import styles from './OrderPad.module.css'

type Modal = { kind: 'nudge' } | { kind: 'pnl' } | { kind: 'tpsl' }

type Props = {
  side?: OrderSide
  product?: OrderProduct
  type?: OrderType
  /** The nudge that fires on Buy / Sell (once — the shared cap is 1 per session). `'options'`: the case and state
   *  picked in the prototype settings. Leave out for an order with no nudge. */
  nudge?: { caseId: NudgeCaseId; state: DataState } | 'options'
  /** Opens with this sheet up (a direct link). */
  initialSheet?: 'nudge' | 'pnl' | 'tpsl'
  /** Today's loss limit, if one is set (null: none yet). */
  limit?: number | null
  /** Lost so far today, against the limit. */
  loss?: number
  onBack: () => void
}

/**
 * The F&O commodity options order pad (fno/order-pad, Figma 8671:18433) — the same screen, copied because flows
 * can't import each other — with the nudge's two layers added:
 * - Silent help (always there): "Set TPSL" becomes a TP/SL row that is pre-filled and switched on; the dock shows how
 *   much of today's loss limit is left (or a link to set one) above Avail. · Req.
 * - The nudge sheet (only when both engines agree): fires on Buy / Sell, once. Not now places the order; a guardrail
 *   opens its sheet in place (sheet → sheet), and saving returns to the pad.
 */
export function OrderPad({ side = 'buy', product: initialProduct = 'delivery', type: initialType = 'market', nudge, initialSheet, limit: initialLimit = null, loss = lossToday, onBack }: Props) {
  const c = orderContract
  const caseValue = useFlowOption('nudges', 'case', 'sizeUp')
  // A stored Layout 5 version ('everyone' … 'compact', removed 19 Nov) isn't a case: show the default case for it.
  const optionCase = (caseById.has(caseValue as NudgeCaseId) ? caseValue : 'sizeUp') as NudgeCaseId
  const optionState = useFlowOption('nudges', 'state', 'A') as DataState
  const actions = useFlowOption('nudges', 'actions', 'one') as 'one' | 'two'
  // Prototype setting: fire the nudge on every Buy / Sell instead of once (the real cap is 1 per session).
  const meterStyle = activeMeter(useFlowOption('nudges', 'meter', 'soft')) as MeterStyle
  const meterLevel = useFlowOption('nudges', 'meterLevel', 'danger') as RiskLevel
  const meterLayout = useFlowOption('nudges', 'meterLayout', 'text') as MeterLayout
  const meterFill = useFlowOption('nudges', 'meterFill', 'whole')
  const meterBlend = meterFill === 'blend' || meterFill === 'split' || meterFill === 'splitWords'
  const meterSplit = meterFill === 'split' || meterFill === 'splitWords'
  // Low · Medium · High round the dial and the risk tag in it: on the 3-block or the one-colour ring.
  const meterWords = meterFill === 'splitWords' || meterFill === 'wholeWords'
  const everyBuy = useFlowOption('nudges', 'everyBuy', 'on') === 'on'
  const vibrate = useFlowOption('nudges', 'vibrate', 'on') === 'on'
  // A screen with a fixed case (03–09) shows it until the case or data state is changed in the settings; from then on
  // it follows the settings too, so the dropdown always changes the sheet that's open.
  const [opened] = useState(`${optionCase}.${optionState}`)
  const changed = `${optionCase}.${optionState}` !== opened
  const picked = nudge === undefined ? undefined
    : nudge === 'options' || changed ? { caseId: optionCase, state: optionState } : nudge
  const fired = picked

  const [product, setProduct] = useState<OrderProduct>(initialProduct)
  const [type, setType] = useState<OrderType>(initialType)
  const [validity, setValidity] = useState(validities[0])
  const [lots, setLots] = useState(false)
  const [quantity, setQuantity] = useState(c.lotSize)
  const [limitPct, setLimitPct] = useState(20.3)
  const [triggerPct, setTriggerPct] = useState(20.3)
  const [tpslOn, setTpslOn] = useState(true)
  const [tpsl, setTpsl] = useState(prefilledTpsl)
  const [limit, setLimit] = useState<number | null>(initialLimit)
  const [modal, setModal] = useState<Modal | null>(initialSheet ? { kind: initialSheet } : null)
  const [nudged, setNudged] = useState(initialSheet !== undefined)
  const [toast, setToast] = useState<string | null>(null)
  const buy = side === 'buy'
  const priceAt = (pct: number) => inr(Math.round(c.ltp * (buy ? 1 - pct / 1000 : 1 + pct / 1000)), 0)
  const priceRow = (label: string, pct: number, setPct: (n: number) => void) => <div className={styles.field}>
    <span className={styles.fieldLabel}>{label}</span>
    <span className={styles.fieldValue}>{priceAt(pct)}</span>
    <Stepper size="sm" className={styles.stepper} label={t('order.offset')} value={pct} onChange={setPct} min={0} step={0.1} format={(n) => `${n.toFixed(1)}%`} />
  </div>

  const place = () => { setModal(null); setToast(t('order.placed', { side: t(buy ? 'order.buy' : 'order.sell'), name: c.name, contract: c.contract })) }
  const submit = () => { if (fired && (!nudged || everyBuy)) { setNudged(true); setModal({ kind: 'nudge' }) } else place() }

  const sheet = modal?.kind === 'nudge' && fired
    ? <NudgeSheet caseId={fired.caseId} state={fired.state} meterStyle={meterStyle} meterLevel={meterLevel} meterLayout={meterLayout} meterBlend={meterBlend} meterSplit={meterSplit} meterWords={meterWords} vibrate={vibrate} actions={actions} onGuardrail={(g) => setModal({ kind: g })} onNotNow={place} />
    : modal?.kind === 'pnl'
      ? <LossLimitSheet initial={limit ?? defaultLimit} onClose={() => setModal(null)} onSave={(amount) => { setLimit(amount); setModal(null); setToast(t('pnl.saved', { amount: inr(amount, 0) })) }} />
      : modal?.kind === 'tpsl'
        ? <TpslSheet onClose={() => setModal(null)} onSave={(v) => { setTpsl(v); setTpslOn(true); setModal(null); setToast(t('tpsl.saved')) }} />
        : undefined

  const top = <Actionbar onBack={onBack} backLabel={t('order.back')}
    title={<span className={styles.contract}>
      <span className={styles.contractName}>{c.name}<Icon icon={lmSwitchArrowVertical} size={16} /><span className={styles.expiry}>{c.contract}</span></span>
      <span className={styles.contractPrice}>{inr(c.ltp, 2)}<PriceChange value={c.change} unit="percent" size="sm" /></span>
    </span>}
    actions={<span className={styles.exchange}>{c.exchange}</span>}
    bottom={<Tabs aria-label={t('order.product')} value={product} onChange={setProduct} items={[{ value: 'delivery', label: t('order.delivery') }, { value: 'intraday', label: t('order.intraday') }]} />} />

  // Silent help in the dock: what's left of today's loss limit, or a link to set one.
  const left = limit !== null ? Math.max(limit - loss, 0) : 0
  const near = limit !== null && left <= limit * 0.2
  const lossLimit = limit !== null
    ? <button type="button" className={styles.limit} onClick={() => setModal({ kind: 'pnl' })}>
      <ProgressBar size="sm" value={(loss / limit) * 100} status={near ? 'warning' : 'default'} label={t('silent.limitBar')} />
      <span className={styles.limitText} data-tone={near ? 'warning' : undefined}><Icon icon={msShield} size={16} />{t('silent.limitLeft', { left: inr(left, 0) })}</span>
    </button>
    : <button type="button" className={styles.limitAdd} onClick={() => setModal({ kind: 'pnl' })}>
      <Icon icon={msShield} size={16} />{t('silent.limitAdd')}<Icon icon={msChevronRight} size={16} />
    </button>

  const dock = <div className={styles.dock} data-surface="primary">
    {product === 'intraday' && <p className={styles.autoClose}>{t('order.autoClose', { time: intradayCloseTime })}</p>}
    <div className={styles.dockBody}>
      {lossLimit}
      <div className={styles.funds}>
        <span><span className={styles.muted}>{t('order.available')}</span> <strong>{inr(funds.available, 0)}</strong></span>
        <span><span className={styles.muted}>{t('order.required')}</span> <strong>{inr(funds.required, 0)}</strong></span>
      </div>
      <Button variant={buy ? 'brand' : 'sell'} size="lg" fullWidth onClick={submit}>{t(buy ? 'order.buy' : 'order.sell')}</Button>
    </div>
  </div>

  return <Screen top={top} bottom={dock} sheet={sheet}
    toast={toast ? <Aerobar floating type="success" heading={toast} onDismiss={() => setToast(null)} /> : undefined}>
    {!SHOWCASE && <SheetSwitcher open={fired !== undefined} />}
    <Stack gap="16" className={styles.body}>
      <Stack direction="horizontal" align="center" justify="between">
        <Tabs aria-label={t('order.type')} appearance="pill" size="md" value={type} onChange={setType} items={(['market', 'limit', 'sl'] as const).map((value) => ({ value, label: t(`order.${value}`) }))} />
        {type !== 'market' && <Select size="sm" aria-label={t('order.validity')} className={styles.validity} onClick={() => setValidity(validities[(validities.indexOf(validity) + 1) % validities.length])}>{validity}</Select>}
      </Stack>
      {!buy && <div className={styles.margin}><span><span className={styles.muted}>{t('order.margin')}</span> <strong>{sellMargin.margin}</strong></span><span><span className={styles.muted}>{t('order.leverage')}</span> <strong>{sellMargin.leverage}</strong></span></div>}
      <div className={styles.field}>
        <button type="button" className={styles.unit} aria-label={t('order.unitSwitch')} onClick={() => setLots(!lots)}>{t(lots ? 'order.lots' : 'order.quantity')}<Icon icon={lmSwitchArrowVertical} size={12} /></button>
        <span className={styles.hint}>{t('order.lotHint', { lot: c.lotSize })}</span>
        <Stepper size="sm" className={styles.stepper} label={t(lots ? 'order.lots' : 'order.quantity')} value={lots ? quantity / c.lotSize : quantity} onChange={(n) => setQuantity(lots ? n * c.lotSize : n)} min={lots ? 1 : c.lotSize} step={lots ? 1 : c.lotSize} />
      </div>
      {type === 'sl' && priceRow(t('order.trigger'), triggerPct, setTriggerPct)}
      {type !== 'market' && priceRow(t('order.limitPrice'), limitPct, setLimitPct)}
      {/* Silent help: the draft's "Set TPSL" link becomes a TP/SL that is already on and pre-filled. */}
      <div className={styles.tpslRow}>
        <button type="button" className={styles.tpslText} aria-label={t('silent.tpslEdit')} onClick={() => setModal({ kind: 'tpsl' })}>
          <span className={styles.tpslHead}>{t('silent.tpsl')}{tpslOn && <Tag size="sm" variant="secondary" color="success">{t('silent.prefilled')}</Tag>}</span>
          <span className={styles.tpslValue}>{tpslOn ? t('silent.tpslOn', { target: tpsl.targetPct, stop: tpsl.stopPct }) : t('silent.tpslOff')}</span>
        </button>
        <Switch aria-label={t('silent.tpslToggle')} checked={tpslOn} onChange={(e) => setTpslOn(e.target.checked)} />
      </div>
    </Stack>
  </Screen>
}
```

### `src/flows/nudges/sheets/BlockDial.module.css`

```css
/* 4g · Blocks (BlockDial.tsx), in the soft dial's design language. SVG drawing units. Resting styles = the end state;
   the component (Web Animations) runs the sweep, the tip's bloom colour and the level word; CSS runs the glows. */
.dial { margin: 0; }
/* The drawing area has room for the whole blurred glow (it reaches ~50 past the ring): 16 past the ring on each side
   (that's the sheet's edge) and 70 above the dial, so nothing draws outside the svg — mobile browsers (Safari) clip
   SVG content at the svg's box, overflow: visible or not, which cut the glow off in a hard edge (19 Nov). The svg is
   that much bigger and pulled back by negative margins (relative to the width, like the viewBox), so the dial sits
   exactly where it did and its box ends at the ring's base line: viewBox −16 −64 372 × 280 for the dial's 340 × 170. */
.svg {
  display: block;
  width: calc(100% * 372 / 340);
  height: auto;
  margin: calc(100% * -70 / 340) calc(100% * -16 / 340) calc(100% * -40 / 340);
  overflow: visible;
}

/* The sweep's resting point (the component animates it). */
.sweepStroke { fill: none; stroke: white; stroke-dasharray: 1; stroke-dashoffset: var(--sweep-rest); }

/* Layer blur per level, as on the soft dial: lighter than the level colour (mixed half with white); on as the fill
   reaches the level, off as it moves on; the last one stays and breathes. */
[data-tone='safe'] { --tone: color-mix(in srgb, var(--l3-surface-accent-success-default) 50%, var(--l3-static-white)); }
[data-tone='caution'] { --tone: color-mix(in srgb, var(--l3-surface-accent-warning-default) 50%, var(--l3-static-white)); }
[data-tone='danger'] { --tone: color-mix(in srgb, var(--l3-surface-accent-error-default) 50%, var(--l3-static-white)); }
.glow { fill: none; stroke: var(--tone); stroke-linecap: round; opacity: 0; animation: glowPass var(--glow-for) linear var(--glow-in) both; }
@keyframes glowPass { 0% { opacity: 0; } 20% { opacity: 0.55; } 70% { opacity: 0.45; } 100% { opacity: 0; } }
.glow[data-last] {
  opacity: 0.35;
  animation:
    fadeIn var(--l3-motion-duration-long) var(--l3-motion-easing-decelerate) var(--glow-in) backwards,
    breathe calc(var(--l3-motion-duration-long) * 4) var(--l3-motion-easing-standard) calc(var(--glow-in) + var(--glow-for)) 2;
}
@keyframes breathe { 50% { opacity: 0.6; } }
@keyframes fadeIn { from { opacity: 0; } }

/* The bloom riding the fill's tip (turns about the ring's centre with the sweep). Resting colour = the reached level. */
.turn { transform-box: view-box; transform-origin: 170px 176px; transform: rotate(var(--to-deg)); }
.bloom { fill: var(--l3-surface-accent-error-default); opacity: 0.4; animation: fadeIn var(--l3-motion-duration-medium) linear var(--bloom-in) backwards; }
.dial[data-level='safe'] .bloom { fill: var(--l3-surface-accent-success-default); }
.dial[data-level='caution'] .bloom { fill: var(--l3-surface-accent-warning-default); }

/* The level word: the reached level's word rests visible; the component swaps them on the way. */
.value { font: var(--l3-text-heading-36); fill: var(--l3-content-primary); animation: fadeIn var(--l3-motion-duration-long) var(--l3-motion-easing-decelerate) var(--word-in) backwards; }
.word { font: var(--l3-text-heading-20); fill: var(--l3-content-primary); opacity: 0; }
.dial[data-level='safe'] .word[data-word='safe'],
.dial[data-level='caution'] .word[data-word='caution'],
.dial[data-level='danger'] .word[data-word='danger'] { opacity: 1; }

@media (prefers-reduced-motion: reduce) {
  .dial * { animation: none !important; }
}
```

### `src/flows/nudges/sheets/BlockDial.tsx`

```tsx
import { useEffect, useId, useRef, type CSSProperties } from 'react'
import { t } from '../copy'
import { easeTimeAt, needleKeys, OVER, RUN, RUN_EASE, type RiskLevel } from './SoftDial'
import styles from './BlockDial.module.css'

/**
 * 4g · Blocks (19 Nov, from the user's reference image; in the soft dial's design language). The half ring as 13 thick
 * rounded blocks with gaps, grey at rest. A fill sweeps in from the left the way the soft dial's needle moves — fast,
 * eased, a little past the reading, back and forth, settling on the last lit block's edge — and the blocks it covers
 * show their level's colour: green (safe), yellow (caution), red (high risk), each with depth (deep inside, light
 * outside; no glass, 19 Nov). The block under the fill's head lifts outward and settles back as it moves on — a wave
 * that travels with the fill. Behind, each level's blurred glow comes on as
 * the fill reaches it; a bloom rides the fill's tip in the colour of the level it's in. In the middle, only the level
 * word, swapping Safe → Caution → High risk as the fill passes. Reduced motion: the end state.
 */
const CX = 170
const CY = 176
const RO = 158
const RI = 104
const RM = (RO + RI) / 2
const BAND = RO - RI
const COUNT = 13
const GAP = 0.018
const ROUND = 7 // corner rounding: each block is drawn this much smaller and stroked this wide, round joins
const SAFE_END = 0.45
const CAUTION_END = 0.65
const LEVEL_AT = { safe: 0.3, caution: 0.56 } as const
/** Fast (19 Nov, "it should fill up fast"): the run to the overshoot takes 0.6 of 1.8 s. */
const DELAY = 250
const TRAVEL = 1800
/** How far the block under the fill's head lifts outward (drawing units). */
const LIFT = 7
/** Progress (0–1) of a cubic-bezier easing at time fraction x. */
function easeTimeInverse(x: number, [x1, y1, x2, y2]: number[]) {
  const b = (s: number, p: number, q: number) => 3 * p * s * (1 - s) ** 2 + 3 * q * s ** 2 * (1 - s) + s ** 3
  let lo = 0, hi = 1
  for (let i = 0; i < 28; i++) { const m = (lo + hi) / 2; if (b(m, x1, x2) < x) lo = m; else hi = m }
  return b((lo + hi) / 2, y1, y2)
}

function point(f: number, r: number) {
  const a = Math.PI * (1 - f)
  return [CX + r * Math.cos(a), CY - r * Math.sin(a)] as const
}
function arc(from: number, to: number, r: number) {
  const [x1, y1] = point(from, r)
  const [x2, y2] = point(to, r)
  return `M ${x1} ${y1} A ${r} ${r} 0 0 1 ${x2} ${y2}`
}
/** One block: an annular sector from f0 to f1, inset by ROUND / 2 so its round stroke brings it back to size. */
function block(f0: number, f1: number) {
  const inset = ROUND / 2
  const ro = RO - inset
  const ri = RI + inset
  const [x1, y1] = point(f0 + inset / (Math.PI * ro), ro)
  const [x2, y2] = point(f1 - inset / (Math.PI * ro), ro)
  const [x3, y3] = point(f1 - inset / (Math.PI * ri), ri)
  const [x4, y4] = point(f0 + inset / (Math.PI * ri), ri)
  return `M ${x1} ${y1} A ${ro} ${ro} 0 0 1 ${x2} ${y2} L ${x3} ${y3} A ${ri} ${ri} 0 0 0 ${x4} ${y4} Z`
}
const toneAt = (f: number) => (f < SAFE_END ? 'safe' : f < CAUTION_END ? 'caution' : 'danger')
const levels = [
  { from: 0, to: SAFE_END, tone: 'safe' },
  { from: SAFE_END, to: CAUTION_END, tone: 'caution' },
  { from: CAUTION_END, to: 1, tone: 'danger' },
] as const
const ms = (v: number) => `${Math.round(v)}ms`
/** Where the reading lands at high risk: 2X at ~75 %, further with a bigger multiple (as the one-colour soft dial). */
const nowAt = (ratio?: number) => (ratio === undefined ? 0.8 : 0.72 + 0.23 * Math.min(1, Math.max(0, (ratio - 1) / 9)))
/** Gradient stops per level: the content colour deep inside, the surface colour through the band, the light tint outside. */
const shades = {
  safe: ['--l3-content-accent-success-default', '--l3-surface-accent-success-default', '--l3-surface-accent-success-light'],
  caution: ['--l3-content-accent-warning-default', '--l3-surface-accent-warning-default', '--l3-surface-accent-warning-light'],
  danger: ['--l3-content-accent-error-default', '--l3-surface-accent-error-default', '--l3-surface-accent-error-light'],
} as const

export function BlockDial({ ratio, level = 'danger', showRatio = false }: { ratio?: number; level?: RiskLevel; showRatio?: boolean }) {
  const number = showRatio && ratio !== undefined
  const uid = useId().replace(/[^a-zA-Z0-9]/g, '')
  const id = (name: string) => `bd-${name}-${uid}`
  const root = useRef<HTMLElement>(null)
  const target = level === 'danger' ? nowAt(ratio) : LEVEL_AT[level]
  const width = (1 - GAP * (COUNT - 1)) / COUNT
  const blocks = Array.from({ length: COUNT }, (_, i) => {
    const f0 = i * (width + GAP)
    return { f0, f1: f0 + width, tone: toneAt(f0 + width / 2), lit: f0 + width / 2 <= target }
  })
  const lit = blocks.filter((b) => b.lit)
  // The fill rests on the last lit block's outer edge, so whole blocks show; it overshoots into the next one and back.
  const end = lit.length ? lit[lit.length - 1].f1 + 0.004 : 0.001
  const END = Math.min(1, end + OVER)
  const reach = (f: number) => DELAY + TRAVEL * RUN * easeTimeAt(Math.min(1, Math.max(0, f / END)), RUN_EASE)
  const amberAt = reach(SAFE_END)
  const redAt = reach(CAUTION_END)
  const label = t(level === 'safe' ? 'nudge.meterLabelSafe' : level === 'caution' ? 'nudge.meterLabelCaution' : 'nudge.meterLabel')
  const words = [['safe', 'nudge.levelSafe'], ['caution', 'nudge.levelMid'], ['danger', 'nudge.highRisk']] as const

  useEffect(() => {
    const el = root.current
    if (!el || window.matchMedia('(prefers-reduced-motion: reduce)').matches) return
    const keys = needleKeys(end)
    const timing = { duration: TRAVEL, delay: DELAY, fill: 'backwards' as const }
    const running: Animation[] = []
    el.querySelectorAll<SVGElement>('[data-turn]').forEach((n) => running.push(n.animate(
      keys.map((k) => ({ offset: k.at, easing: k.easing, transform: `rotate(${(k.f - 0.5) * 180}deg)` })), timing)))
    const sweep = el.querySelector<SVGElement>('[data-sweep]')
    if (sweep) running.push(sweep.animate(keys.map((k) => ({ offset: k.at, easing: k.easing, strokeDashoffset: `${1 - Math.min(k.f, END) / END}` })), timing))

    // Keyframe offsets must never go backwards: at this speed one level can pass in less than two fades.
    const forward = <T extends { offset: number }>(keys: T[]) => { let last = 0; return keys.map((k) => ({ ...k, offset: (last = Math.max(last, k.offset)) })) }
    // The tip's bloom takes the colour of the level the fill is in (oklab blends at each edge).
    const total = DELAY + TRAVEL
    const at = (v: number) => Math.min(1, Math.max(0, v / total))
    const css = getComputedStyle(el)
    const c = (name: string) => css.getPropertyValue(name).trim()
    const g = c('--l3-surface-accent-success-default'), y = c('--l3-surface-accent-warning-default'), r = c('--l3-surface-accent-error-default')
    const mix = (a: string, b: string) => `color-mix(in oklab, ${a}, ${b})`
    const w = 120
    const bloomKeys = level === 'safe' ? [{ offset: 0, fill: g }, { offset: 1, fill: g }]
      : level === 'caution' ? [{ offset: 0, fill: g }, { offset: at(amberAt - w), fill: g }, { offset: at(amberAt), fill: mix(g, y) }, { offset: at(amberAt + w), fill: y }, { offset: 1, fill: y }]
        : [{ offset: 0, fill: g }, { offset: at(amberAt - w), fill: g }, { offset: at(amberAt), fill: mix(g, y) }, { offset: at(amberAt + w), fill: y },
          { offset: at(Math.max(amberAt + w, redAt - w)), fill: y }, { offset: at(redAt), fill: mix(y, r) }, { offset: at(redAt + w), fill: r }, { offset: 1, fill: r }]
    el.querySelectorAll<SVGElement>('[data-bloom]').forEach((n) => running.push(n.animate(forward(bloomKeys), { duration: total, fill: 'backwards' })))

    // The level word swaps as the fill passes each edge (out, then in).
    const fade = 90
    const word = (tone: string, from: number, to: number) => el.querySelectorAll<SVGElement>(`[data-word="${tone}"]`).forEach((n) => running.push(n.animate(forward([
      { offset: 0, opacity: from <= 0 ? 1 : 0 }, { offset: at(from), opacity: from <= 0 ? 1 : 0 }, { offset: at(from + fade), opacity: 1 },
      { offset: at(to - fade), opacity: 1 }, { offset: at(to), opacity: to >= total ? 1 : 0 }, { offset: 1, opacity: to >= total ? 1 : 0 },
    ]), { duration: total, fill: 'backwards' })))
    if (level === 'caution') { word('safe', 0, amberAt); word('caution', amberAt, total) }
    if (level === 'danger') { word('safe', 0, amberAt); word('caution', amberAt, redAt); word('danger', redAt, total) }
    // The wave (19 Nov): the block under the fill's head lifts outward (LIFT) and settles back as the head moves on, so a
    // wave travels with the fill — over the overshoot and back-swing too. The head's path is sampled from the same keys.
    const samples = 90
    const headAt = (p: number) => {
      const i = keys.findIndex((k, n) => n < keys.length - 1 && p >= k.at && p <= keys[n + 1].at)
      const a = keys[Math.max(0, i)], b = keys[Math.max(0, i) + 1]
      const local = (p - a.at) / (b.at - a.at || 1)
      const m = /cubic-bezier\(([^)]+)\)/.exec(a.easing)
      const e = m ? easeTimeInverse(local, m[1].split(',').map(Number)) : local
      return a.f + (b.f - a.f) * e
    }
    const heads = Array.from({ length: samples + 1 }, (_, n) => headAt(n / samples))
    el.querySelectorAll<SVGGElement>('[data-block]').forEach((n) => {
      const b = blocks[Number(n.dataset.block)]
      const mid = (b.f0 + b.f1) / 2
      const reachWidth = width * 0.9
      const a = Math.PI * (1 - mid)
      const lifts = heads.map((h) => Math.max(0, 1 - Math.abs(h - mid) / reachWidth))
      if (!lifts.some((l) => l > 0)) return
      running.push(n.animate(lifts.map((l, k) => ({ offset: k / samples, transform: `translate(${(Math.cos(a) * LIFT * l).toFixed(2)}px, ${(-Math.sin(a) * LIFT * l).toFixed(2)}px)` })), { duration: TRAVEL, delay: DELAY, fill: 'backwards' }))
    })
    return () => running.forEach((a) => a.cancel())
  }, [end, END, amberAt, redAt, level]) // blocks / width follow from level and ratio, as end does

  const big = { x: -80, y: -80, width: 500, height: 400 }
  const shape = (b: (typeof blocks)[number], i: number, paint: string, extra?: string) =>
    <path key={`${extra ?? 'b'}${i}`} d={block(b.f0, b.f1)} fill={paint} stroke={paint} strokeWidth={ROUND} strokeLinejoin="round" className={extra} />
  return <figure ref={root} className={styles.dial} data-level={level} role="img" aria-label={label}
    style={{ '--to-deg': `${(end - 0.5) * 180}deg`, '--sweep-rest': 1 - end / END, '--bloom-in': ms(DELAY), '--word-in': ms(reach(end)) } as CSSProperties}>
    <svg viewBox="-16 -64 372 280" className={styles.svg} aria-hidden>
      <defs>
        {(['safe', 'caution', 'danger'] as const).map((tone) =>
          <radialGradient key={tone} id={id(tone)} gradientUnits="userSpaceOnUse" cx={CX} cy={CY} r={RO}>
            <stop offset={RI / RO} style={{ stopColor: `var(${shades[tone][0]})` }} />
            <stop offset={(RI + BAND * 0.45) / RO} style={{ stopColor: `var(${shades[tone][1]})` }} />
            <stop offset="1" style={{ stopColor: `var(${shades[tone][2]})` }} />
          </radialGradient>)}
        {/* Unlit blocks: a soft grey, a touch lighter outside. */}
        <radialGradient id={id('grey')} gradientUnits="userSpaceOnUse" cx={CX} cy={CY} r={RO}>
          <stop offset={RI / RO} style={{ stopColor: 'var(--l3-surface-tertiary)' }} />
          <stop offset="1" style={{ stopColor: 'var(--l3-surface-secondary)' }} />
        </radialGradient>
        <mask id={id('sweep')} maskUnits="userSpaceOnUse" {...big}>
          <path d={arc(0, END, RM)} pathLength={1} className={styles.sweepStroke} strokeWidth={BAND + 30} data-sweep />
        </mask>
        <filter id={id('blur')} filterUnits="userSpaceOnUse" {...big}><feGaussianBlur stdDeviation="14" /></filter>
      </defs>

      {/* Layer blur: each level's glow comes on as the fill reaches it and fades as it moves on; the last one stays. */}
      {levels.filter((l) => l.from < end).map((l) => {
        const last = end <= l.to
        const on = l.from === 0 ? DELAY : reach(l.from)
        const off = reach(Math.min(l.to, end)) + 250
        return <path key={l.tone} d={arc(l.from + 0.02, Math.min(l.to, end) - 0.01, RM)} className={styles.glow} data-tone={l.tone}
          data-last={last || undefined} strokeWidth={BAND + 16} filter={`url(#${id('blur')})`}
          style={{ '--glow-in': ms(on), '--glow-for': ms(off - on) } as CSSProperties} />
      })}

      {/* Each block: grey, with its gradient over it where the sweep has reached (no glass, 19 Nov). Each block is its own
          group so it can lift outward as the fill passes it — the wave. */}
      {blocks.map((b, i) => <g key={i} data-block={i}>
        {shape(b, i, `url(#${id('grey')})`)}
        <g mask={`url(#${id('sweep')})`}>{shape(b, i, `url(#${id(b.tone)})`, 'c')}</g>
      </g>)}

      {/* The bloom riding the fill's tip. */}
      <g className={styles.turn} data-turn>
        <circle cx={CX} cy={CY - RM} r={BAND / 2} className={styles.bloom} data-bloom filter={`url(#${id('blur')})`} />
      </g>

      {number && <text x={CX} y={CY - 38} className={styles.value} textAnchor="middle">{ratio}X</text>}
      {words.map(([tone, key]) => <text key={tone} x={CX} y={number ? CY - 6 : CY - 22} className={styles.word} data-word={tone} textAnchor="middle">{t(key)}</text>)}
    </svg>
  </figure>
}
```

### `src/flows/nudges/sheets/GaugeMeter.module.css`

```css
/* 4h / 4i · the speedometer (GaugeMeter.tsx), from Figma LM → F&O Playground 6917:62779 (20 Nov). The dial's colours
   are Figma's illustration; the labels and the wave are L3 tokens. */

/* Above the wave (which is positioned too, and comes first). */
.gauge { position: relative; display: block; overflow: visible; }

/* The scale: L3/Heading/12 in content/primary. */
.label { font: var(--l3-text-heading-12); fill: var(--l3-content-primary); }

/* The parts that move turn / scale in the dial's own drawing units (origin = the needle's hub). */
.part, .needle { transform-box: view-box; }

/* The wave behind the top of the sheet: the level's light tint, blurred. It never takes a tap. */
.wave {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: var(--l3-size-96);
  overflow: visible;
  filter: blur(var(--app-gauge-wave-blur));
  pointer-events: none;
}
.wave[data-level='danger'] { fill: var(--l3-surface-accent-indicator-down-light); }
.wave[data-level='caution'] { fill: var(--l3-surface-accent-warning-light); }
.wave[data-level='safe'] { fill: var(--l3-surface-accent-indicator-up-light); }
```

### `src/flows/nudges/sheets/GaugeMeter.tsx`

```tsx
import { useEffect, useId, useRef, type CSSProperties } from 'react'
import { t } from '../copy'
import { GAUGE_H, GAUGE_W, NEEDLE_DRAWN_AT, NEEDLE_PIVOT, face, needle, ring, segments } from './gaugeArt'
import { needleKeys, RUN_EASE, easeTimeAt, RUN } from './SoftDial'
import type { RiskLevel } from './SoftDial'
import styles from './GaugeMeter.module.css'

/** 4h (0 · 30 · 50 · 80 · 100) and 4i (Low · Medium · High): the speedometer from Figma (LM → F&O Playground
 *  6917:62779, 20 Nov). */
export type GaugeScale = 'numbers' | 'words'

/**
 * The scale around the dial, as Figma lays it out: each label's centre (x) and top (y) in the 103-tall group, and
 * where the dial sits in it. Figma geometry, in drawing units (1 = 1 px at 1×).
 */
const SCALES = {
  numbers: {
    width: 213.83, dialX: 36.78,
    labels: [{ text: '0', x: 14, y: 79 }, { text: '30', x: 35.92, y: 32 }, { text: '50', x: 106.71, y: 0 }, { text: '80', x: 178.92, y: 32 }, { text: '100', x: 202.83, y: 78 }],
  },
  words: {
    width: 214.66, dialX: 34.62,
    labels: [{ key: 'gauge.low', x: 12, y: 78 }, { key: 'gauge.medium', x: 105.04, y: 0 }, { key: 'gauge.high', x: 200.66, y: 78 }],
  },
} as const
const DIAL_Y = 24.38
const HEIGHT = DIAL_Y + GAUGE_H // 103

/** Where the needle stops, as a fraction of the scale: high risk where Figma draws it (78), medium straight up, low on
 *  the green. */
export const GAUGE_AT: Record<RiskLevel, number> = { danger: NEEDLE_DRAWN_AT / 100, caution: 0.5, safe: 0.15 }
/** The needle as drawn, turned to fraction f of the scale (0 = pointing left, 1 = right). */
const turn = (f: number) => `rotate(${(f - NEEDLE_DRAWN_AT / 100) * 180}deg)`

/** Motion (once, on open): the six colour segments light up left → right with their scale labels, then the needle
 *  swings up from 0 with the soft dial's run (ease in-out a little past the reading, back a touch, settle). */
const SEGMENT_IN = 260
const SEGMENT_STAGGER = 70
const NEEDLE_DELAY = 320
const NEEDLE_TRAVEL = 2000

export function GaugeMeter({ scale, level = 'danger' }: { scale: GaugeScale; level?: RiskLevel }) {
  const root = useRef<SVGSVGElement>(null)
  const uid = useId()
  const s = SCALES[scale]
  const now = GAUGE_AT[level]
  const label = t(level === 'safe' ? 'nudge.meterLabelSafe' : level === 'caution' ? 'nudge.meterLabelCaution' : 'nudge.meterLabel')

  useEffect(() => {
    const el = root.current
    if (!el || window.matchMedia('(prefers-reduced-motion: reduce)').matches) return
    const running: Animation[] = []
    el.querySelectorAll<SVGGElement>('[data-segment]').forEach((n) => {
      const i = Number(n.dataset.segment)
      running.push(n.animate([{ opacity: 0, transform: 'scale(0.92)' }, { opacity: 1, transform: 'none' }],
        { duration: SEGMENT_IN, delay: i * SEGMENT_STAGGER, easing: 'cubic-bezier(0.2, 0, 0, 1)', fill: 'backwards' }))
    })
    // A label comes in with the segment under it (by where it sits along the dial).
    el.querySelectorAll<SVGTextElement>('[data-label]').forEach((n) => {
      const at = Number(n.dataset.label)
      running.push(n.animate([{ opacity: 0 }, { opacity: 1 }], { duration: SEGMENT_IN, delay: at * 5 * SEGMENT_STAGGER, easing: 'ease-out', fill: 'backwards' }))
    })
    const pointer = el.querySelector<SVGGElement>('[data-needle]')
    if (pointer) running.push(pointer.animate(
      needleKeys(now).map((k) => ({ offset: k.at, easing: k.easing, transform: turn(k.f) })),
      { duration: NEEDLE_TRAVEL, delay: NEEDLE_DELAY, fill: 'backwards' }))
    // The segment the needle lands on gives a small pulse as it arrives.
    const landing = el.querySelector<SVGGElement>(`[data-segment="${Math.min(5, Math.floor(now * 6))}"] [data-band]`)
    const arrive = NEEDLE_DELAY + NEEDLE_TRAVEL * RUN * easeTimeAt(1 - 0.02, RUN_EASE)
    if (landing) running.push(landing.animate([{ filter: 'brightness(1)' }, { filter: 'brightness(1.25)' }, { filter: 'brightness(1)' }],
      { duration: 500, delay: arrive, easing: 'ease-in-out' }))
    return () => running.forEach((a) => a.cancel())
  }, [now, scale])

  return (
    <svg ref={root} className={styles.gauge} width={s.width} height={HEIGHT} viewBox={`0 0 ${s.width} ${HEIGHT}`} role="img" aria-label={label} data-gauge={uid}>
      {s.labels.map((l) => (
        <text key={l.x} x={l.x} y={l.y + 8} className={styles.label} textAnchor="middle" dominantBaseline="central" data-label={l.x / s.width}>
          {'text' in l ? l.text : t(l.key)}
        </text>
      ))}
      <svg x={s.dialX} y={DIAL_Y} width={GAUGE_W} height={GAUGE_H} viewBox={`0 0 ${GAUGE_W} ${GAUGE_H}`} overflow="visible">
        {segments.map((seg, i) => (
          <g key={i} data-segment={i} style={{ transformOrigin: `${NEEDLE_PIVOT.x}px ${NEEDLE_PIVOT.y}px` }} className={styles.part}>
            <path d={seg.rim.d} fill={seg.rim.fill} />
            <path d={seg.band.d} fill={seg.band.fill} data-band />
          </g>
        ))}
        <path d={ring.d} fill={ring.fill} />
        <path d={face.d} fill={face.fill} />
        <g data-needle className={styles.needle} style={{ transformOrigin: `${NEEDLE_PIVOT.x}px ${NEEDLE_PIVOT.y}px`, transform: turn(now) } as CSSProperties}>
          <path d={needle.d} fill={needle.fill} />
        </g>
      </svg>
    </svg>
  )
}

/**
 * The soft pink wave behind the top of the sheet (Figma "Ellipse 1 / 2": the level's light tint, blurred 11.3), drawn
 * in the 360-wide frame it was designed in and stretched to the sheet's width. 4h and 4i place it a little apart.
 */
const WAVE = { numbers: { x: -20, y: -43 }, words: { x: -17, y: -56 } } as const
export function GaugeWave({ scale, level = 'danger' }: { scale: GaugeScale; level?: RiskLevel }) {
  const w = WAVE[scale]
  return (
    <svg className={styles.wave} data-level={level} viewBox="0 0 360 96" preserveAspectRatio="none" overflow="visible" aria-hidden="true">
      <path transform={`translate(${w.x - 22.6} ${w.y - 22.6})`} d="M415.6 89.1133C415.6 125.848 411.184 132.915 214.684 113.447C106.16 113.447 22.6 209.162 22.6 89.1133C22.6 52.379 110.576 22.6 219.1 22.6C327.624 22.6 415.6 52.379 415.6 89.1133Z" />
    </svg>
  )
}
```

### `src/flows/nudges/sheets/GuardrailSheets.module.css`

```css
/* Loss limit and TP/SL sheets. */
.body { padding: 0; }

/* 24 more above the content (L3's 16 top space + 24, as since L3 2.3; 2.4 made the 16 the content's own padding).
   Same in NudgeSheet. */
.sheet [data-sheet-content] { padding-top: calc(var(--l3-spacing-16) + var(--l3-spacing-24)); }
```

### `src/flows/nudges/sheets/GuardrailSheets.tsx`

```tsx
import { useId, useState } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { KeyValueList, KeyValueRow } from '@l3/components/KeyValueList'
import { Stack } from '@l3/components/Stack'
import { Stepper } from '@l3/components/Stepper'
import { Tabs } from '@l3/components/Tabs'
import { TextField } from '@l3/components/TextField'
import { inr } from '../../../shared/data/format'
import { Sheet } from '../../../shared/ui/Sheet'
import { t } from '../copy'
import { limitPresets, orderContract, prefilledTpsl } from '../data'
import styles from './GuardrailSheets.module.css'

/**
 * "Today's loss limit" (PnL Protect, set from the nudge or the order pad). Amount field (focused, as with the keypad
 * up) with quick amounts as pill tabs under it; the dock saves it. Pre-filled with a sample amount so one tap sets it.
 */
export function LossLimitSheet({ initial, onSave, onClose }: { initial: number; onSave: (amount: number) => void; onClose: () => void }) {
  const id = useId()
  const [amount, setAmount] = useState(initial)
  const preset = limitPresets.includes(amount) ? String(amount) : ''
  return <Sheet labelledBy={id} onClose={onClose} className={styles.sheet}
    header={<BottomSheetHeader headingId={id} heading={t('pnl.title')} description={t('pnl.description')} />}
    footer={<ButtonGroup aria-label={t('pnl.title')}><Button size="lg" disabled={amount <= 0} onClick={() => onSave(amount)}>{t('pnl.save')}</Button></ButtonGroup>}>
    <Stack gap="16" className={styles.body}>
      <TextField label={t('pnl.field')} inputMode="numeric" autoFocus value={amount ? inr(amount, 0) : ''}
        onChange={(e) => setAmount(Number(e.target.value.replace(/[^\d]/g, '')) || 0)} helperText={t('pnl.helper')} helperIcon={false} />
      <Tabs aria-label={t('pnl.presets')} appearance="pill" value={preset} onChange={(v) => setAmount(Number(v))}
        items={limitPresets.map((n) => ({ value: String(n), label: inr(n, 0) }))} />
    </Stack>
  </Sheet>
}

/**
 * "TP/SL for this order": target profit and stop-loss as % of the entry price (steppers), the price each one exits
 * at beside it. Opens with the order pad's pre-filled values.
 */
export function TpslSheet({ onSave, onClose }: { onSave: (tpsl: { targetPct: number; stopPct: number }) => void; onClose: () => void }) {
  const id = useId()
  const [targetPct, setTarget] = useState(prefilledTpsl.targetPct)
  const [stopPct, setStop] = useState(prefilledTpsl.stopPct)
  const at = (pct: number) => t('tpsl.atPrice', { price: inr(orderContract.ltp * (1 + pct / 100)) })
  const pct = (n: number) => `${n}%`
  return <Sheet labelledBy={id} onClose={onClose} className={styles.sheet}
    header={<BottomSheetHeader headingId={id} heading={t('tpsl.title')} description={t('tpsl.description')} />}
    footer={<ButtonGroup aria-label={t('tpsl.title')}><Button size="lg" onClick={() => onSave({ targetPct, stopPct })}>{t('tpsl.save')}</Button></ButtonGroup>}>
    <div className={styles.body}>
      <KeyValueList>
        <KeyValueRow label={t('tpsl.target')} caption={at(targetPct)} value={<Stepper size="sm" label={t('tpsl.target')} value={targetPct} onChange={setTarget} min={1} step={1} format={pct} />} />
        <KeyValueRow label={t('tpsl.stop')} caption={at(-stopPct)} value={<Stepper size="sm" label={t('tpsl.stop')} value={stopPct} onChange={setStop} min={1} max={99} step={1} format={pct} />} />
      </KeyValueList>
    </div>
  </Sheet>
}
```

### `src/flows/nudges/sheets/NudgeSheet.module.css`

```css
/* The overtrading nudge — the risk meter sheet (NudgeSheet.tsx), on L3 tokens. Everything is centred. */

/* 24 above the content, as these sheets have looked since L3 2.3 (L3's 16 top space + 24; 2.4 made the 16 the content's
   own padding). Same in GuardrailSheets. */
.sheet [data-sheet-content] { padding-top: calc(var(--l3-spacing-16) + var(--l3-spacing-24)); }
/* 32 from the last text to the dock's line (19 Nov, user; L3's sheet content has 24). */
.meterSheet.meterSheet [data-sheet-content] { padding-bottom: var(--l3-spacing-32); }

.riskText { text-align: center; }
/* The text is drawn over the meter's glow, which now reaches up behind it (the dial's drawing area is taller). */
.riskText h2, .riskText p, .overtradingTag { position: relative; z-index: 1; }

/* "You are overtrading": a step up from the sheet header's 20 (19 Nov, user). */
.meterHeading { margin: 0; font: var(--l3-text-heading-24); color: var(--l3-content-primary); }
/* "Your chances of losing money today are high" under it. */
.meterNote { margin: 0; font: var(--l3-text-label-14); color: var(--l3-content-secondary); }
/* The fact under the meter, as body text in secondary. */
.meterFact { margin: 0; font: var(--l3-text-description-14); color: var(--l3-content-secondary); }

/* Tag first: the risk as the heading, 18. */
.meterRiskHeading { margin: 0; font: var(--l3-text-heading-18); color: var(--l3-content-primary); }
/* Tag first · the "Overtrading Detected" tag (19 Nov, user): 8 in from each side and 2 between the icon and the label.
   L3's Medium Tag has 2 at the sides and 6 icon → text (its label's own 4 padding); no prop for this (L3 gap). */
.overtradingTag.overtradingTag[data-size] { --tag-padding-inline: var(--l3-spacing-08); --tag-label-padding: 0; }

/* 4h / 4i · the speedometer layout (Figma 6917:62779): 24 from the sheet's top and 24 from the fact to the dock's line
   (Figma: the header's 24 top and 8 bottom + the sheet's 16 top space). The wave is drawn from the content's top. */
.gaugeSheet.gaugeSheet [data-sheet-content] {
  position: relative;
  padding-top: var(--l3-spacing-24);
  padding-bottom: var(--l3-spacing-24);
}
.gaugeHeading { margin: 0; font: var(--l3-text-heading-20); color: var(--l3-content-primary); }
/* The fact: L3/Description/14 (20 Nov, user; Figma had Label/12). */
.gaugeSub { margin: 0; font: var(--l3-text-description-14); color: var(--l3-content-secondary); }
/* Soft dial · 3 blocks · Low–High: the speedometer's 24 from the fact to the dock's line. */
.wordsSheet.wordsSheet [data-sheet-content] { padding-bottom: var(--l3-spacing-24); }
```

### `src/flows/nudges/sheets/NudgeSheet.tsx`

```tsx
import { useEffect, useId } from 'react'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msWarningFill } from '@l3/icons/material'
import { Sheet } from '../../../shared/ui/Sheet'
import copy from '../copy.json'
import { t } from '../copy'
import { caseById, type DataState, type Guardrail, type NudgeCaseId } from '../data'
import styles from './NudgeSheet.module.css'
import { SoftDial, type RiskLevel } from './SoftDial'
import { BlockDial } from './BlockDial'
import { GaugeMeter, GaugeWave } from './GaugeMeter'

/** The meter (prototype setting "Meter"): 4f Soft dial (default), 4g Blocks, 4h / 4i the speedometer (0–100 / Low–High). */
export type MeterStyle = 'soft' | 'blocks' | 'gauge' | 'gaugeWords'
/** Where the text sits around the meter (prototype setting "Meter layout"). */
export type MeterLayout = 'text' | 'meter' | 'tag' | 'number'

export type NudgeSheetProps = {
  caseId: NudgeCaseId
  state: DataState
  /** 4f Soft dial (default) or 4g Blocks. */
  meterStyle?: MeterStyle
  /** Where the reading stops: safe, caution or high risk (default). */
  meterLevel?: RiskLevel
  /** Text first (default) · Meter first · Tag first · With multiple. */
  meterLayout?: MeterLayout
  /** Soft dial only: the blended green → yellow → red fill (default: one colour for the whole ring). */
  meterBlend?: boolean
  /** Soft dial, blended only: the ring cut into three blocks where the colour changes. */
  meterSplit?: boolean
  /** Soft dial (3 blocks or one colour): Low · Medium · High round the dial, the risk tag in it. */
  meterWords?: boolean
  /** Buzz the phone once when the sheet opens (two short pulses: "stop, look"). */
  vibrate?: boolean
  /** 'one' (default): the case's guardrail (primary) + Not now (secondary) · 'two': both guardrails + Not now (ghost). */
  actions?: 'one' | 'two'
  onGuardrail: (g: Guardrail) => void
  /** Not now: the order goes ahead (on Positions: just closes). Also a scrim tap, drag down or Esc. */
  onNotNow: () => void
}

type Key = Parameters<typeof t>[0]

/**
 * The overtrading nudge — the risk meter (19 Nov, cut down to the versions in use). ONE sheet for every case; a case only
 * fills the slots. It opens on a trader who is mid-order and stressed: what this is, the meter that shows how far from
 * his usual he is, and one plain line on what he is doing — then the guardrail, or Not now.
 *
 * Meter layouts (all centred, 24 between blocks, the body text 16 under the meter, 32 to the dock's line):
 * - Text first: "You are overtrading" (heading-24) over "Your chances of losing money today are high", the meter, the fact.
 * - Meter first: the meter, then "You are overtrading" and the fact.
 * - Tag first: a red "Overtrading Detected" tag (filled warning icon), "Your chances of losing money are high"
 *   (heading-18), the meter with the multiple in it, the fact.
 * - With multiple: as Text first (since 20 Nov no version shows the multiple in the dial, so the two now match).
 * The fact: size-up says "You are placing a bigger order right after a loss"; every other case its own fact
 * ("3X more orders than usual today"). Motion plays once on open; none with reduced motion.
 */
export function NudgeSheet({ caseId, state, meterStyle = 'soft', meterLevel = 'danger', meterLayout = 'text', meterBlend = false, meterSplit = false, meterWords = false, vibrate = true, actions = 'one', onGuardrail, onNotNow }: NudgeSheetProps) {
  const id = useId()
  // On open only (switching case or version keeps the sheet open, so it doesn't buzz again).
  useEffect(() => { if (vibrate) buzz() }, []) // eslint-disable-line react-hooks/exhaustive-deps
  const nudge = caseById.get(caseId)!
  const key = (slot: string) => `case.${caseId}.${slot}` as Key
  const has = (slot: string) => key(slot) in copy

  // C (thin history): no "usual" to compare with → today's raw fact, no multiple.
  const thin = state === 'C' && has('raw')
  const headline = t(key(thin ? 'raw' : 'headline'))
  const ratio = thin ? undefined : nudge.ratio
  const fact = caseId === 'sizeUp' ? t('nudge.biggerAfterLoss') : headline

  const other: Guardrail = nudge.primary === 'pnl' ? 'tpsl' : 'pnl'
  const label = (g: Guardrail) => t(g === 'pnl' ? 'nudge.setPnl' : 'nudge.setTpsl')
  const notNow = t(nudge.where === 'positions' ? 'nudge.notNowPositions' : 'nudge.notNow')
  const footer = <ButtonGroup aria-label={headline} direction="vertical">
    <Button size="lg" onClick={() => onGuardrail(nudge.primary)}>{label(nudge.primary)}</Button>
    {actions === 'two' && <Button size="lg" variant="secondary" onClick={() => onGuardrail(other)}>{label(other)}</Button>}
    <Button size="lg" variant={actions === 'two' ? 'ghost' : 'secondary'} onClick={onNotNow}>{notNow}</Button>
  </ButtonGroup>

  // No multiple ("2X") in the dial in any layout (20 Nov): the disc shows the level word only.
  // The speedometer's text under a meter: the risk tag, 16, the heading (heading-20), 8, the fact (description-14).
  const tagColor = meterLevel === 'danger' ? 'loss' : meterLevel === 'caution' ? 'warning' : 'profit'
  const riskTag = <Tag size="md" variant="secondary" color={tagColor} iconLeft={meterLevel === 'safe' ? undefined : <Icon icon={msWarningFill} />}>{t(`gauge.tag.${meterLevel}`)}</Tag>
  const headingAndFact = <Stack gap="08" align="center">
    <h2 id={id} className={styles.gaugeHeading}>{t(`gauge.heading.${meterLevel}`)}</h2>
    <p className={styles.gaugeSub}>{caseId === 'sizeUp' ? t('gauge.sizeUp') : headline}</p>
  </Stack>
  const meter = meterStyle === 'blocks'
    ? <BlockDial key={`${caseId}-${state}-${meterLevel}-${meterLayout}`} ratio={ratio} level={meterLevel} />
    : <SoftDial key={`${caseId}-${state}-${meterLevel}-${meterBlend}-${meterSplit}-${meterWords}-${meterLayout}`} ratio={ratio} level={meterLevel} blend={meterBlend} split={meterSplit} words={meterWords} badge={meterWords ? riskTag : undefined} />
  const meterAndFact = <Stack gap="16" align="center">
    {meter}
    <p className={styles.meterFact}>{fact}</p>
  </Stack>

  // 4h / 4i · the speedometer (Figma 6917:62779, 20 Nov) has its own layout, whatever the Meter layout setting: the pink
  // wave at the top, the dial with its scale, 16, the risk tag, 16, "Your chances of losing money is high" (heading-20), 8, the
  // fact (label-12). 24 from the top of the sheet, 24 from the fact to the dock's line.
  const gaugeText = <Stack gap="16" align="center">{riskTag}{headingAndFact}</Stack>
  if (meterStyle === 'gauge' || meterStyle === 'gaugeWords') {
    const scale = meterStyle === 'gauge' ? 'numbers' : 'words'
    return <Sheet labelledBy={id} onClose={onNotNow} className={`${styles.sheet} ${styles.gaugeSheet}`} footer={footer}>
      <GaugeWave scale={scale} level={meterLevel} />
      <Stack gap="16" align="center" className={styles.riskText}>
        <GaugeMeter key={`${caseId}-${state}-${meterLevel}-${scale}`} scale={scale} level={meterLevel} />
        {gaugeText}
      </Stack>
    </Sheet>
  }
  // Soft dial · 3 blocks · Low–High (20 Nov, user): the speedometer's layout under the dial (no word in the dial), and
  // the speedometer's 24 to the dock's line; Meter layout doesn't apply.
  if (meterWords) {
    return <Sheet labelledBy={id} onClose={onNotNow} className={`${styles.sheet} ${styles.wordsSheet}`} footer={footer}>
      {/* The risk tag sits in the dial (where the level word was) and fades in as the needle lands; 24 from the dial to
          the heading (20 Nov, user). */}
      <Stack gap="24" align="center" className={styles.riskText}>
        {meter}
        {headingAndFact}
      </Stack>
    </Sheet>
  }

  let body
  if (meterLayout === 'meter') {
    // Meter first: other cases add the risk to their fact ("3X more orders than usual today. Your chances … are high").
    const line = caseId === 'sizeUp' ? fact : t('nudge.meterFirstSubOther', { fact: headline })
    body = <Stack gap="24" align="center" className={styles.riskText}>
      {meter}
      <Stack gap="04" align="center">
        <h2 id={id} className={styles.meterHeading}>{t('nudge.meterHeading')}</h2>
        <p className={styles.meterFact}>{line}</p>
      </Stack>
    </Stack>
  } else if (meterLayout === 'tag') {
    body = <Stack gap="24" align="center" className={styles.riskText}>
      <Stack gap="12" align="center">
        <Tag size="md" variant="secondary" color="error" className={styles.overtradingTag} iconLeft={<Icon icon={msWarningFill} />}>{t('nudge.tag')}</Tag>
        <h2 id={id} className={styles.meterRiskHeading}>{t('nudge.meterRiskHeading')}</h2>
      </Stack>
      {meterAndFact}
    </Stack>
  } else {
    // Text first and With multiple (the same since the dial's multiple was removed, 20 Nov).
    body = <Stack gap="24" align="center" className={styles.riskText}>
      <Stack gap="04" align="center">
        <h2 id={id} className={styles.meterHeading}>{t('nudge.meterHeading')}</h2>
        <p className={styles.meterNote}>{t('nudge.meterSub')}</p>
      </Stack>
      {meterAndFact}
    </Stack>
  }

  return <Sheet labelledBy={id} onClose={onNotNow} className={`${styles.sheet} ${styles.meterSheet}`} footer={footer}>{body}</Sheet>
}

/** Two short pulses. In the Lemonn Android app the page calls its native bridge (works without a tap, and uses the
 *  phone's haptics); in a browser, the Vibration API (Android Chrome only, and only right after a tap; iPhone: none). */
const BUZZ = [70, 60, 70]
function buzz() {
  const app = (window as { LemonnApp?: { vibrate?: (pattern: string) => void } }).LemonnApp
  try {
    if (app?.vibrate) app.vibrate(BUZZ.join(','))
    else navigator.vibrate?.(BUZZ)
  } catch { /* no vibration here */ }
}
```

### `src/flows/nudges/sheets/SoftDial.module.css`

```css
/* 4f · Soft dial (SoftDial.tsx). SVG drawing units, not CSS px. Resting styles are the END state (red / High risk);
   the component (Web Animations) runs the needle, sweep, colours and level word from green before it. CSS keeps only
   the timed fades: --travel-in / --travel-for (the needle's run) and each glow's --glow-in / --glow-for. */

/* The drawing area has room for the whole blurred glow (it reaches ~50 past the ring): 16 past the ring on each side
   (that's the sheet's edge) and 70 above the dial, so nothing draws outside the svg — mobile browsers (Safari) clip
   SVG content at the svg's box, overflow: visible or not, which cut the glow off in a hard edge (19 Nov). The svg is
   that much bigger and pulled back by negative margins (relative to the width, like the viewBox), so the dial sits
   exactly where it did and its box ends at the ring's base line: viewBox −16 −64 372 × 280 for the dial's 340 × 170. */
.svg {
  display: block;
  width: calc(100% * 372 / 340);
  height: auto;
  margin: calc(100% * -70 / 340) calc(100% * -16 / 340) calc(100% * -40 / 340);
  overflow: visible;
}

.dial { position: relative; margin: 0; }
/* Resting colours = the end state (red); the component animates them from green. */
.deep { stop-color: var(--l3-content-accent-error-default); }
.mid { stop-color: var(--l3-surface-accent-error-default); }
.light { stop-color: var(--l3-surface-accent-error-light); }

/* The sweep's resting point (the component animates it). */
.sweepStroke { fill: none; stroke: white; stroke-dasharray: 1; stroke-dashoffset: var(--sweep-rest); }

.dot { fill: var(--l3-static-white); }
/* Grains left on the fill: quiet; the component fades the green and yellow ones away as the needle moves on. */
.dotsPassed { opacity: 0.45; }
.dotsPassed[data-grains='safe'], .dotsPassed[data-grains='caution'] { opacity: 0; }
/* …except on the level the reading stops in. */
.dial[data-level='safe'] .dotsPassed[data-grains='safe'], .dial[data-level='caution'] .dotsPassed[data-grains='caution'] { opacity: 0.45; }
/* The grains in the soft wedge riding behind the needle: a little brighter than the rest (kept gentle, 19 Nov). */
.dots { opacity: 0.6; }

.rest { fill: var(--l3-surface-accent-error-light); opacity: 0.5; }

.tick { stroke: var(--l3-content-primary); stroke-width: 2; stroke-linecap: round; opacity: 0.12; }
.tick[data-filled] { opacity: 0.3; }

/* Bloom at the tip: comes in as the fill lands. */
.bloom { fill: var(--l3-surface-accent-error-default); opacity: 0.45; animation: fadeIn var(--l3-motion-duration-long) var(--l3-motion-easing-decelerate) calc(var(--travel-in) + var(--travel-for) - var(--l3-motion-duration-medium)) backwards; }

/* Layer blur per level: on as the fill reaches the level, off as it moves on; the last one stays and breathes. */
/* Lighter than the level colour itself: the level colour mixed half with white. */
[data-tone='safe'] { --tone: color-mix(in srgb, var(--l3-surface-accent-success-default) 50%, var(--l3-static-white)); }
[data-tone='caution'] { --tone: color-mix(in srgb, var(--l3-surface-accent-warning-default) 50%, var(--l3-static-white)); }
[data-tone='danger'] { --tone: color-mix(in srgb, var(--l3-surface-accent-error-default) 50%, var(--l3-static-white)); }
.glow { fill: none; stroke: var(--tone); stroke-linecap: round; opacity: 0; animation: glowPass var(--glow-for) linear var(--glow-in) both; }
@keyframes glowPass { 0% { opacity: 0; } 20% { opacity: 0.6; } 70% { opacity: 0.5; } 100% { opacity: 0; } }
.glow[data-last] {
  opacity: 0.35;
  animation:
    fadeIn var(--l3-motion-duration-long) var(--l3-motion-easing-decelerate) var(--glow-in) backwards,
    breathe calc(var(--l3-motion-duration-long) * 4) var(--l3-motion-easing-standard) calc(var(--glow-in) + var(--glow-for)) 2;
}
@keyframes breathe { 50% { opacity: 0.6; } }

/* The needle and the grain wedge turn about the ring's centre (170, 176). */
.turn { transform-box: view-box; transform-origin: 170px 176px; transform: rotate(var(--to-deg)); }
/* The needle is black in both modes. */
.needle { fill: var(--l3-static-black); }
/* Glass on the fill. */
.glassLine { fill: none; stroke: var(--l3-static-white); stroke-linecap: round; opacity: 0.55; }
.glassRim { fill: none; stroke: var(--l3-static-white); stroke-width: 1; stroke-opacity: 0.5; }

.disc { fill: var(--l3-surface-primary); }
.level { font: var(--l3-text-heading-20); fill: var(--l3-content-primary); }
/* "With multiple": the multiple above the word (L3's largest heading, 36), in once the reading lands. */
.value { font: var(--l3-text-heading-36); fill: var(--l3-content-primary); animation: fadeIn var(--l3-motion-duration-long) var(--l3-motion-easing-decelerate) calc(var(--travel-in) + var(--travel-for)) backwards; }

/* The level word: only the reached level's word rests visible; the component swaps Safe → Caution → High risk on the way. */
.level { opacity: 0; }
.dial[data-level='safe'] .level[data-word='safe'],
.dial[data-level='caution'] .level[data-word='caution'],
.dial[data-level='danger'] .level[data-word='danger'] { opacity: 1; }

/* Resting colours of the safe and caution versions (danger's are the defaults above). */
.dial[data-level='safe'] .deep { stop-color: var(--l3-content-accent-success-default); }
.dial[data-level='safe'] .mid { stop-color: var(--l3-surface-accent-success-default); }
.dial[data-level='safe'] .light { stop-color: var(--l3-surface-accent-success-light); }
.dial[data-level='safe'] .rest { fill: var(--l3-surface-accent-success-light); }
.dial[data-level='safe'] .bloom { fill: var(--l3-surface-accent-success-default); }
.dial[data-level='caution'] .deep { stop-color: var(--l3-content-accent-warning-default); }
.dial[data-level='caution'] .mid { stop-color: var(--l3-surface-accent-warning-default); }
.dial[data-level='caution'] .light { stop-color: var(--l3-surface-accent-warning-light); }
.dial[data-level='caution'] .rest { fill: var(--l3-surface-accent-warning-light); }
.dial[data-level='caution'] .bloom { fill: var(--l3-surface-accent-warning-default); }
@keyframes fadeIn { from { opacity: 0; } }

@media (prefers-reduced-motion: reduce) {
  .dial, .dial * { animation: none !important; }
}

/* "3 blocks · Low–High": the scale round the dial, as on the 4i speedometer (L3 heading, content/primary; in drawing
   units the dial's 340 is about the content's width, so 14 ≈ 14 px). */
/* 16 (20 Nov, user: 2 bigger again). */
.scaleWord { font: var(--l3-text-heading-16); fill: var(--l3-content-primary); }

/* 3 blocks · Low–High: the risk Tag in the disc, centred where the level word sat (top set by the component). */
.badge { position: absolute; left: 50%; translate: -50% -50%; }
```

### `src/flows/nudges/sheets/SoftDial.tsx`

```tsx
import { useEffect, useId, useRef, type CSSProperties, type ReactNode } from 'react'
import { t } from '../copy'
import styles from './SoftDial.module.css'

/**
 * 4f · Soft dial (Anshul_Risk_Meter, 19 Nov; from the user's reference image): a thick half ring filled from the left
 * end to the reading with a soft radial gradient (deep at the inner edge, light at the outer), a dotted texture and
 * small ticks on the filled part, a short dark marker at its tip, and a white disc in the middle holding the number,
 * the level and a sub line. The unfilled rest is a faint tint that fades out, with a blurred bloom at the tip.
 * The colour follows the level the fill is in: green → yellow → red as it sweeps (gradient stops animate), the level
 * word in the disc changes with it ("Low risk" → "Caution" → "High risk"), and each level gets a blurred glow behind
 * it as the fill reaches it. The lower ends fade into the sheet, as in the reference.
 * Reduced motion: the end state.
 */
const CX = 170
const CY = 176
const RO = 160 // outer edge of the ring
const RI = 96 // inner edge = the disc
const RM = (RO + RI) / 2
const BAND = RO - RI
/** Level edges of the BLENDED fill (19 Nov, user): yellow starts further left and red is kept to the right — green
 *  0–33 %, yellow 33–60 %, red 60–100 % of the ring. The one-colour fill keeps the earlier 45 % / 65 % (19 Nov, user:
 *  "a little less, like the previous version"). */
const SAFE_END = 0.33
const CAUTION_END = 0.6
/** "3 blocks · Low–High" (20 Nov, user): three equal blocks, so Medium sits over the middle of the yellow one. */
const EDGES = { blend: { safe: SAFE_END, caution: CAUTION_END }, whole: { safe: 0.45, caution: 0.65 }, thirds: { safe: 1 / 3, caution: 2 / 3 } } as const
const DELAY = 300
/** The needle's run (19 Nov): ease-in-out up to a little past the reading (fast in the middle), then back a touch,
 *  forward a touch, and it settles — a soft bounce. Offsets are of TRAVEL; f = fraction of the scale. */
const TRAVEL = 2400
export const OVER = 0.05
export const RUN = 0.6
export const RUN_EASE = [0.42, 0, 0.3, 1] as const
export const needleKeys = (now: number) => [
  { at: 0, f: 0, easing: `cubic-bezier(${RUN_EASE.join(',')})` },
  { at: RUN, f: Math.min(1, now + OVER), easing: 'cubic-bezier(0.45, 0, 0.55, 1)' },
  { at: 0.79, f: now - 0.022, easing: 'cubic-bezier(0.45, 0, 0.55, 1)' },
  { at: 0.91, f: now + 0.007, easing: 'cubic-bezier(0.3, 0, 0.3, 1)' },
  { at: 1, f: now, easing: 'linear' },
]
/** Time fraction (0–1) at which a cubic-bezier easing reaches progress p. */
export function easeTimeAt(p: number, [x1, y1, x2, y2]: readonly number[]) {
  const b = (s: number, a: number, c: number) => 3 * a * s * (1 - s) ** 2 + 3 * c * s ** 2 * (1 - s) + s ** 3
  let lo = 0, hi = 1
  for (let i = 0; i < 30; i++) { const m = (lo + hi) / 2; if (b(m, y1, y2) < p) lo = m; else hi = m }
  return b((lo + hi) / 2, x1, x2)
}

function point(f: number, r: number) {
  const a = Math.PI * (1 - f)
  return [CX + r * Math.cos(a), CY - r * Math.sin(a)] as const
}
function arc(from: number, to: number, r: number) {
  const [x1, y1] = point(from, r)
  const [x2, y2] = point(to, r)
  return `M ${x1} ${y1} A ${r} ${r} 0 0 1 ${x2} ${y2}`
}
/** An annular sector of the ring, from f0 to f1. */
function sector(f0: number, f1: number) {
  const [ox0, oy0] = point(f0, RO)
  const [ox1, oy1] = point(f1, RO)
  const [ix1, iy1] = point(f1, RI)
  const [ix0, iy0] = point(f0, RI)
  return `M ${ox0} ${oy0} A ${RO} ${RO} 0 0 1 ${ox1} ${oy1} L ${ix1} ${iy1} A ${RI} ${RI} 0 0 0 ${ix0} ${iy0} Z`
}
const ms = (v: number) => `${Math.round(v)}ms`
const levelsFor = (e: { safe: number; caution: number }) => [
  { from: 0, to: e.safe, tone: 'safe' },
  { from: e.safe, to: e.caution, tone: 'caution' },
  { from: e.caution, to: 1, tone: 'danger' },
] as const

/** Where the reading stops (19 Nov): in the green, yellow or red level. Danger lands by the multiple, as before. */
export type RiskLevel = 'safe' | 'caution' | 'danger'
/** Where the needle stops. Blended: well into the red (2X at ~85 %); one colour: as before (2X at ~75 %). */
const LEVEL_AT = { blend: { safe: 0.2, caution: 0.47 }, whole: { safe: 0.3, caution: 0.56 } } as const
const multiple = (ratio?: number) => Math.min(1, Math.max(0, ((ratio ?? 1) - 1) / 9))
const dangerAt = (ratio: number | undefined, blend: boolean) => blend
  ? (ratio === undefined ? 0.86 : 0.84 + 0.12 * multiple(ratio))
  : (ratio === undefined ? 0.8 : 0.72 + 0.23 * multiple(ratio))

/**
 * Blended fill (19 Nov, user): each part of the ring keeps the colour it was filled with — green, then yellow, then red —
 * melting into each other over a soft band at each level edge, so it reads as one ramp, not three blocks. Drawn as thin
 * slices along the arc (SVG has no conic gradient), each in its own oklab mix of the level colours, under a radial shade
 * that keeps the ring's depth (darker inside, lighter outside).
 */
const BLEND = 0.07 // half-width of the soft band at each level edge, as a fraction of the scale
const SLICES = 90
function blendColour(f: number, split = false, edges: { safe: number; caution: number } = EDGES.blend) {
  const g = 'var(--l3-surface-accent-success-default)'
  const y = 'var(--l3-surface-accent-warning-default)'
  const r = 'var(--l3-surface-accent-error-default)'
  const mix = (a: string, b: string, t: number) => `color-mix(in oklab, ${a} ${Math.round((1 - t) * 100)}%, ${b})`
  const ease = (t: number) => t * t * (3 - 2 * t) // smoothstep, so each band starts and ends softly
  if (split) return f < edges.safe ? g : f < edges.caution ? y : r // three blocks: each its own colour, cut at the edges
  if (f <= SAFE_END - BLEND) return g
  if (f < SAFE_END + BLEND) return mix(g, y, ease((f - (SAFE_END - BLEND)) / (2 * BLEND)))
  if (f <= CAUTION_END - BLEND) return y
  if (f < CAUTION_END + BLEND) return mix(y, r, ease((f - (CAUTION_END - BLEND)) / (2 * BLEND)))
  return r
}

/** "Blended · 3 blocks" (20 Nov, user): the gap cut across the ring at each colour change, in drawing units. */
const CUT = 6

/** "3 blocks · Low–High" (20 Nov, user): the dial drawn at this size about its base centre, so Low · Medium · High fit
 *  round it — Low and High beside the ends, Medium over the top — as on the 4i speedometer. */
const WORDS_SCALE = 0.84
const WORDS_GAP = 18 // from the ring to a scale word, in drawing units

/** The badge in the disc (3 blocks · Low–High): its centre, in drawing units, where the level word sat. The figure's box
 *  runs from y 6 to the base line (CY) and x 0–340 (see .svg in the CSS). */
const BADGE_Y = CY - 26 * WORDS_SCALE
const BADGE_FADE = 300

export function SoftDial({ ratio, level = 'danger', blend = false, split = false, words = false, badge, showRatio = false }: { ratio?: number; level?: RiskLevel; blend?: boolean; split?: boolean; words?: boolean; badge?: ReactNode; showRatio?: boolean }) {
  const uid = useId().replace(/[^a-zA-Z0-9]/g, '')
  const ids = { fill: `sd-fill-${uid}`, sweep: `sd-sweep-${uid}`, fade: `sd-fade-${uid}`, fadeMask: `sd-fademask-${uid}`, dots: `sd-dots-${uid}`, trail: `sd-trail-${uid}`, trailBlur: `sd-trailblur-${uid}`, blur: `sd-blur-${uid}`, glass: `sd-glass-${uid}`, shadow: `sd-shadow-${uid}`, shade: `sd-shade-${uid}`, cut: `sd-cut-${uid}` }
  const root = useRef<HTMLElement>(null)
  const number = showRatio && ratio !== undefined
  // Three equal blocks wherever the ring is cut into blocks or carries the Low · Medium · High scale (20 Nov, user).
  const edges = EDGES[words || split ? 'thirds' : blend ? 'blend' : 'whole']
  const levels = levelsFor(edges)
  // Equal thirds: low and medium stop in the middle of their block; high as the blended dial.
  const now = level === 'danger' ? dangerAt(ratio, blend) : words || split ? { safe: 1 / 6, caution: 0.5 }[level] : LEVEL_AT[blend ? 'blend' : 'whole'][level]
  // The fill is drawn to END (a little past the reading) so the overshoot has colour under it; the sweep shows f / END of it.
  const END = Math.min(1, now + OVER)
  // When the needle first reaches fraction f (during its run).
  const reach = (f: number) => DELAY + TRAVEL * RUN * easeTimeAt(Math.min(1, Math.max(0, f / END)), RUN_EASE)
  const amberAt = reach(edges.safe)
  const redAt = reach(edges.caution)
  const timing = { '--travel-in': ms(DELAY), '--travel-for': ms(TRAVEL * RUN), '--to-deg': `${(now - 0.5) * 180}deg`, '--sweep-rest': 1 - now / END } as CSSProperties
  const label = t(level === 'safe' ? 'nudge.meterLabelSafe' : level === 'caution' ? 'nudge.meterLabelCaution' : 'nudge.meterLabel')
  const [tipX, tipY] = point(now, RM)

  // The motion, in one place (Web Animations), so the needle, the fill, the grains, the colours and the level word all
  // follow the same eased, bouncing path. Reduced motion: none — the CSS end state shows.
  useEffect(() => {
    const el = root.current
    if (!el || window.matchMedia('(prefers-reduced-motion: reduce)').matches) return
    const keys = needleKeys(now)
    const timingOpts = { duration: TRAVEL, delay: DELAY, fill: 'backwards' as const }
    const running: Animation[] = []
    el.querySelectorAll<SVGElement>('[data-turn]').forEach((n) => running.push(n.animate(
      keys.map((k) => ({ offset: k.at, easing: k.easing, transform: `rotate(${(k.f - 0.5) * 180}deg)` })), timingOpts)))
    const sweep = el.querySelector<SVGElement>('[data-sweep]')
    if (sweep) running.push(sweep.animate(keys.map((k) => ({ offset: k.at, easing: k.easing, strokeDashoffset: `${1 - Math.min(k.f, END) / END}` })), timingOpts))

    // Colours: green → yellow → red as the needle crosses each level, each change spread over a short window and
    // passing through an oklab mix (no muddy midpoint). Driven on the elements themselves (gradient stops, tints).
    const css = getComputedStyle(el)
    const tone = (name: string) => css.getPropertyValue(name).trim()
    const total = DELAY + TRAVEL
    // Green holds until just before the yellow level, blends to full yellow at the middle of the yellow level, then
    // blends on to red just past it — one continuous change, each blend at least 0.25 s (oklab: no muddy midpoint).
    const win = 250
    const yellowAt = (amberAt + redAt) / 2
    const at = (v: number) => Math.min(1, Math.max(0, v / total))
    const shift = (g: string, y: string, r: string) => {
      const mix = (a: string, b: string) => `color-mix(in oklab, ${a}, ${b})`
      // Safe: green throughout. Caution: green, then blends to yellow as the needle enters it, and stays yellow.
      if (level === 'safe') return [{ offset: 0, c: g }, { offset: 1, c: g }]
      if (level === 'caution') return [{ offset: 0, c: g }, { offset: at(amberAt - win), c: g }, { offset: at(amberAt), c: mix(g, y) }, { offset: at(amberAt + win), c: y }, { offset: 1, c: y }]
      const gEnd = Math.min(amberAt, yellowAt - win)
      const rStart = Math.max(redAt, yellowAt + win)
      return [
        { offset: 0, c: g }, { offset: at(gEnd), c: g }, { offset: at((gEnd + yellowAt) / 2), c: mix(g, y) }, { offset: at(yellowAt), c: y },
        { offset: at((yellowAt + rStart) / 2), c: mix(y, r) }, { offset: at(rStart), c: r }, { offset: 1, c: r },
      ]
    }
    const deep = shift(tone('--l3-content-accent-success-default'), tone('--l3-content-accent-warning-default'), tone('--l3-content-accent-error-default'))
    const mid = shift(tone('--l3-surface-accent-success-default'), tone('--l3-surface-accent-warning-default'), tone('--l3-surface-accent-error-default'))
    const light = shift(tone('--l3-surface-accent-success-light'), tone('--l3-surface-accent-warning-light'), tone('--l3-surface-accent-error-light'))
    const colourOpts = { duration: total, fill: 'backwards' as const }
    const paint = (sel: string, prop: 'stopColor' | 'fill', keysC: { offset: number; c: string }[]) =>
      el.querySelectorAll<SVGElement>(sel).forEach((n) => running.push(n.animate(keysC.map((k) => ({ offset: k.offset, [prop]: k.c })), colourOpts)))
    paint('[data-stop="deep"]', 'stopColor', deep)
    paint('[data-stop="mid"]', 'stopColor', mid)
    paint('[data-stop="light"]', 'stopColor', light)
    // The ring behind starts plain grey; its tint only comes in as the needle starts (0.25 s), then follows the colour.
    const grey = tone('--l3-surface-tertiary')
    const tintIn = DELAY + 250
    paint('[data-paint="light"]', 'fill', [{ offset: 0, c: grey }, { offset: at(DELAY), c: grey }, { offset: at(tintIn), c: light[0].c }, ...light.filter((k) => k.offset > at(tintIn))])
    paint('[data-paint="mid"]', 'fill', mid)

    // Grains left on the green and yellow levels fade away once the needle leaves the level (over 0.5 s); red keeps them.
    el.querySelectorAll<SVGElement>('[data-grains]').forEach((n) => {
      const level = levels.find((l) => l.tone === n.dataset.grains)!
      if (level.tone === 'danger' || level.to >= now) return
      const leave = reach(level.to)
      running.push(n.animate([{ offset: 0, opacity: 0.45 }, { offset: at(leave), opacity: 0.45 }, { offset: at(leave + 500), opacity: 0 }, { offset: 1, opacity: 0 }], colourOpts))
    })

    // The level word cross-fades with the colour.
    // The level word swaps (0.1 s out, 0.1 s in) where the colour turns: Low risk → Caution (entering yellow) → High risk (entering red).
    const fade = 100
    const word = (tone: string, from: number, to: number) => el.querySelectorAll<SVGElement>(`[data-word="${tone}"]`).forEach((n) => running.push(n.animate([
      // Out, then in (never both at once): the old word fades out just before the change, the new one in just after.
      { offset: 0, opacity: from <= 0 ? 1 : 0 }, { offset: at(from), opacity: from <= 0 ? 1 : 0 }, { offset: at(from + fade), opacity: 1 },
      { offset: at(to - fade), opacity: 1 }, { offset: at(to), opacity: to >= total ? 1 : 0 }, { offset: 1, opacity: to >= total ? 1 : 0 },
    ], colourOpts)))
    const toYellow = (Math.min(amberAt, yellowAt - win) + yellowAt) / 2
    const toRed = (yellowAt + Math.max(redAt, yellowAt + win)) / 2
    if (level === 'caution') {
      word('safe', 0, amberAt)
      word('caution', amberAt, total)
    } else if (level === 'danger') {
      word('safe', 0, toYellow)
      word('caution', toYellow, toRed)
      word('danger', toRed, total)
    }
    // The badge in the disc fades in as the needle reaches the reading (not before: it names where the needle lands).
    const badgeEl = el.querySelector<HTMLElement>('[data-badge]')
    if (badgeEl) running.push(badgeEl.animate([{ opacity: 0, scale: 0.9 }, { opacity: 1, scale: 1 }],
      { duration: BADGE_FADE, delay: reach(now), easing: 'cubic-bezier(0.2, 0, 0, 1)', fill: 'backwards' }))
    return () => running.forEach((a) => a.cancel())
  }, [now, END, amberAt, redAt, level])

  const big = { x: -80, y: -80, width: 500, height: 400 }
  return <figure ref={root} className={styles.dial} data-level={level} data-blend={blend || undefined} style={timing} role="img" aria-label={label}>
    <svg viewBox="-16 -64 372 280" className={styles.svg} aria-hidden>
      {words && <>
        <text x={CX - RO * WORDS_SCALE - WORDS_GAP} y={CY - 14} className={styles.scaleWord} textAnchor="end" dominantBaseline="central">{t('gauge.low')}</text>
        <text x={CX} y={CY - RO * WORDS_SCALE - WORDS_GAP} className={styles.scaleWord} textAnchor="middle" dominantBaseline="central">{t('gauge.medium')}</text>
        <text x={CX + RO * WORDS_SCALE + WORDS_GAP} y={CY - 14} className={styles.scaleWord} textAnchor="start" dominantBaseline="central">{t('gauge.high')}</text>
      </>}
      <g transform={words ? `translate(${CX} ${CY}) scale(${WORDS_SCALE}) translate(${-CX} ${-CY})` : undefined}>
      <defs>
        {/* Deep at the inner edge, the level colour through the band, light at the outer edge. */}
        <radialGradient id={ids.fill} gradientUnits="userSpaceOnUse" cx={CX} cy={CY} r={RO}>
          <stop offset={RI / RO} className={styles.deep} data-stop="deep" />
          <stop offset={(RI + BAND * 0.45) / RO} className={styles.mid} data-stop="mid" />
          <stop offset="1" className={styles.light} data-stop="light" />
        </radialGradient>
        {/* The sweep: a band-wide stroke drawn from the left end, following the needle. */}
        <mask id={ids.sweep} maskUnits="userSpaceOnUse" {...big}>
          <path d={arc(0, END, RM)} pathLength={1} className={styles.sweepStroke} strokeWidth={BAND + 4} data-sweep />
        </mask>
        {/* The lower ends fade into the sheet. The mask reaches well past the ring, so the blur is never cut. */}
        <linearGradient id={ids.fade} gradientUnits="userSpaceOnUse" x1="0" y1={CY - RO} x2="0" y2={CY}>
          <stop offset="0.55" stopColor="white" />
          <stop offset="1" stopColor="white" stopOpacity="0" />
        </linearGradient>
        <mask id={ids.fadeMask} maskUnits="userSpaceOnUse" {...big}>
          <rect {...big} fill={`url(#${ids.fade})`} />
        </mask>
        <pattern id={ids.dots} patternUnits="userSpaceOnUse" width="19" height="19">
          <circle cx="9.5" cy="9.5" r="1.1" className={styles.dot} />
        </pattern>
        <filter id={ids.blur} filterUnits="userSpaceOnUse" {...big}><feGaussianBlur stdDeviation="14" /></filter>
        {/* Glass on the fill: a sheen bright at the outer edge, clearing toward the middle of the band. */}
        {/* Blended fill's depth: a little darker at the inner edge, lighter at the outer, over the coloured slices. */}
        <radialGradient id={ids.shade} gradientUnits="userSpaceOnUse" cx={CX} cy={CY} r={RO}>
          <stop offset={RI / RO} stopColor="black" stopOpacity="0.14" />
          <stop offset={(RI + BAND * 0.45) / RO} stopColor="black" stopOpacity="0" />
          <stop offset="1" stopColor="white" stopOpacity="0.4" />
        </radialGradient>
        <radialGradient id={ids.glass} gradientUnits="userSpaceOnUse" cx={CX} cy={CY} r={RO}>
          <stop offset={RI / RO} stopColor="white" stopOpacity="0.18" />
          <stop offset={(RI + BAND * 0.55) / RO} stopColor="white" stopOpacity="0" />
          <stop offset={(RO - 10) / RO} stopColor="white" stopOpacity="0.28" />
          <stop offset="1" stopColor="white" stopOpacity="0.55" />
        </radialGradient>
        {/* The grains travel with the needle: a soft wedge just behind it, turning with it, lifts them; the rest stay quiet. */}
        <filter id={ids.trailBlur} filterUnits="userSpaceOnUse" {...big}><feGaussianBlur stdDeviation="7" /></filter>
        <mask id={ids.trail} maskUnits="userSpaceOnUse" {...big}>
          <g className={styles.turn} data-turn>
            <path d={sector(0.4, 0.5)} fill="white" filter={`url(#${ids.trailBlur})`} />
          </g>
        </mask>
        <filter id={ids.shadow} filterUnits="userSpaceOnUse" {...big}><feGaussianBlur stdDeviation="1.5" /></filter>
        {/* 3 blocks: everything but the gaps — a straight cut across the band at each colour change. */}
        {split && <mask id={ids.cut} maskUnits="userSpaceOnUse" {...big}>
          <rect {...big} fill="white" />
          {[edges.safe, edges.caution].map((f) => {
            const [x1, y1] = point(f, RI - 4)
            const [x2, y2] = point(f, RO + 4)
            return <line key={f} x1={x1} y1={y1} x2={x2} y2={y2} stroke="black" strokeWidth={CUT} />
          })}
        </mask>}
      </defs>

      <g mask={`url(#${ids.fadeMask})`}>
        {/* Layer blur: a glow of each level's colour behind it, on as the needle reaches it, off as it moves on. */}
        {levels.filter((l) => l.from < now).map((l) => {
          const last = now <= l.to
          const on = l.from === 0 ? DELAY : reach(l.from)
          const off = reach(Math.min(l.to, now)) + 300
          return <path key={l.tone} d={arc(l.from + 0.02, Math.min(l.to, now) - 0.01, RM)} className={styles.glow} data-tone={l.tone}
            data-last={last || undefined} strokeWidth={BAND + 16} filter={`url(#${ids.blur})`}
            style={{ '--glow-in': ms(on), '--glow-for': ms(off - on) } as CSSProperties} />
        })}

        {/* The unfilled rest: a faint tint of the current colour. */}
        <path d={sector(0, 1)} className={styles.rest} data-paint="light" mask={split ? `url(#${ids.cut})` : undefined} />

        {/* The fill, its glass and grains, revealed by the sweep. */}
        <g mask={split ? `url(#${ids.cut})` : undefined}><g mask={`url(#${ids.sweep})`}>
          {blend
            ? <>
              {Array.from({ length: SLICES }, (_, i) => {
                const from = (i / SLICES) * END
                const to = Math.min(END, ((i + 1) / SLICES) * END + 0.003) // a hair of overlap: no seams between slices
                return <path key={i} d={sector(from, to)} style={{ fill: blendColour((from + to) / 2, split, edges) }} />
              })}
              <path d={sector(0, END)} fill={`url(#${ids.shade})`} />
            </>
            : <path d={sector(0, END)} fill={`url(#${ids.fill})`} />}
          <path d={sector(0, END)} fill={`url(#${ids.glass})`} />
          <path d={arc(0.02, END - 0.01, RO - 9)} className={styles.glassLine} strokeWidth={3} filter={`url(#${ids.shadow})`} />
          <path d={sector(0, END)} className={styles.glassRim} />
          {/* The grains the needle leaves on each level: they fade away from the green and yellow levels once the needle
              has moved on; only the red level keeps a few. */}
          {levels.map((l) => <path key={l.tone} d={sector(l.from, Math.min(l.to, END))} fill={`url(#${ids.dots})`}
            className={styles.dotsPassed} data-grains={l.tone} />)}
          <g mask={`url(#${ids.trail})`}>
            <path d={sector(0, END)} fill={`url(#${ids.dots})`} className={styles.dots} />
          </g>
        </g></g>



        {/* Ticks near the inner edge, all the way round; quieter on the unfilled part. */}
        {Array.from({ length: 13 }, (_, i) => {
          const f = (i + 0.5) / 13
          const [x1, y1] = point(f, RI + 12)
          const [x2, y2] = point(f, RI + 20)
          return <line key={i} x1={x1} y1={y1} x2={x2} y2={y2} className={styles.tick} data-filled={f <= now || undefined} />
        })}

        {/* A soft bloom at the tip. */}
        <circle cx={tipX} cy={tipY} r={BAND / 2} className={styles.bloom} data-paint="mid" filter={`url(#${ids.blur})`} />
      </g>

      {/* The needle: solid black, standing on the disc (its foot tucks under it), most of the way across the band. */}
      <g className={styles.turn} data-turn>
        <rect x={CX - 3} y={CY - RO + 18} width={6} height={RO - 18 - RI + 8} rx={3} className={styles.needle} />
      </g>

      {/* The disc: the multiple and the level word (no sub line, 19 Nov). */}
      <circle cx={CX} cy={CY} r={RI} className={styles.disc} />
      {/* "With multiple" (19 Nov): the multiple, big, above the level word. */}
      {number && <text x={CX} y={CY - 38} className={styles.value} textAnchor="middle">{ratio}X</text>}
      {/* 3 blocks · Low–High: no word in the dial (the risk is in the tag under it). */}
      {!words && (['safe', 'caution', 'danger'] as const).map((tone) =>
        <text key={tone} x={CX} y={number ? CY - 6 : CY - 22} className={styles.level} data-word={tone} textAnchor="middle">
          {/* 3 blocks · Low–High: the word follows the scale — Low risk · Medium risk · High risk. */}
          {t(tone === 'safe' ? (words ? 'nudge.levelLow' : 'nudge.levelSafe') : tone === 'caution' ? (words ? 'nudge.levelMedium' : 'nudge.levelMid') : 'nudge.highRisk')}
        </text>)}
      </g>
    </svg>
    {badge && <div className={styles.badge} data-badge style={{ top: `${((BADGE_Y - 6) / (CY - 6)) * 100}%` }}>{badge}</div>}
  </figure>
}
```

### `src/flows/nudges/sheets/gaugeArt.ts`

```ts
// GENERATED from the Figma speedometer (LM → F&O Playground 6916:62677 "_x2014_ÎÓÈ_x5F_3", 20 Nov): its paths and
// fills exactly as exported, in a 141.435 × 78.6169 box. A brand illustration (like L3's selected nav icons): its
// colours are Figma's own, not tokens. Split so the parts can move: the six segments left → right (thin rim + thick
// band), the black ring, the dark face and the white needle (pivot = the hub's centre).
export const GAUGE_W = 141.435
export const GAUGE_H = 78.6169
export const NEEDLE_PIVOT = { x: 70.7165, y: 68.9279 }
/** The needle as drawn points 39° above the right horizontal: 78.3 on a 0–100 scale. */
export const NEEDLE_DRAWN_AT = 78.3
export const segments: { rim: { d: string; fill: string }; band: { d: string; fill: string } }[] = [
  { rim: { d: 'M9.47328 35.2943L10.6659 35.9811C4.76477 46.1627 1.38627 57.9803 1.38627 70.5896H0C0 57.7234 3.43846 45.6646 9.45391 35.2841L9.47328 35.2943Z', fill: '#97FF64' }, band: { d: 'M22.211 69.0994C22.2156 68.9521 22.2221 68.8057 22.2276 68.6584C22.2415 68.2956 22.2599 67.9347 22.2821 67.5748C22.2913 67.4265 22.2996 67.2774 22.3106 67.1292C22.342 66.6799 22.3789 66.2315 22.4232 65.785C22.4269 65.7454 22.4296 65.7058 22.4333 65.6672C22.4822 65.1792 22.5412 64.694 22.6049 64.2097C22.6215 64.0809 22.6408 63.952 22.6593 63.8231C22.7109 63.4576 22.7653 63.093 22.8253 62.7302C22.8474 62.5931 22.8696 62.4559 22.8936 62.3196C22.9673 61.8924 23.0457 61.468 23.1306 61.0454C23.1407 60.9957 23.149 60.946 23.1592 60.8963C23.257 60.4157 23.363 59.9379 23.4746 59.4628C23.4986 59.3616 23.5244 59.2612 23.5484 59.1599C23.6397 58.7825 23.7356 58.4068 23.8362 58.0321C23.8685 57.9134 23.8998 57.7937 23.933 57.6749C24.0474 57.2606 24.1673 56.8472 24.2927 56.4375C24.3075 56.3897 24.3213 56.3409 24.3361 56.293C24.4818 55.8207 24.6349 55.3521 24.7954 54.8853C24.8185 54.8181 24.8424 54.7518 24.8664 54.6846C25.0029 54.2915 25.145 53.902 25.2916 53.5135C25.3294 53.4132 25.3673 53.3137 25.406 53.2143C25.5609 52.8111 25.7214 52.4096 25.8874 52.0119C25.9059 51.9677 25.9234 51.9235 25.9419 51.8793C26.1346 51.419 26.3348 50.9624 26.5423 50.5094C26.5571 50.4781 26.5718 50.4468 26.5866 50.4146C26.7729 50.0086 26.9657 49.6062 27.163 49.2067C27.2045 49.1229 27.246 49.04 27.2875 48.9562C27.4812 48.5705 27.6786 48.1875 27.8815 47.8073C27.9055 47.7631 27.9286 47.718 27.9526 47.6728C28.1905 47.2309 28.4349 46.7936 28.6858 46.36L10.6671 35.9805C4.76602 46.1611 1.3866 57.9806 1.3866 70.5881H22.1861C22.1861 70.09 22.1963 69.5947 22.211 69.0994Z', fill: '#7BDB44' } },
  { rim: { d: 'M35.3587 9.45586L36.0468 10.6463C25.5248 16.7226 16.7534 25.4781 10.666 35.981L9.47347 35.2942L9.4541 35.2831C15.6605 24.5648 24.6099 15.6316 35.3477 9.43652L35.3587 9.45586Z', fill: '#E2FF64' }, band: { d: 'M28.6829 46.3626C28.9283 45.9391 29.1819 45.5202 29.4402 45.1049C29.4854 45.0322 29.5315 44.9595 29.5767 44.8867C29.8229 44.4945 30.0738 44.106 30.3311 43.7212C30.3477 43.6963 30.3643 43.6715 30.3809 43.6466C30.6659 43.2231 30.9565 42.8042 31.2544 42.3899C31.2784 42.3568 31.3033 42.3236 31.3272 42.2896C31.5929 41.9222 31.8631 41.5595 32.1389 41.1995C32.1906 41.1323 32.2422 41.0651 32.2939 40.9979C32.9035 40.2107 33.5362 39.442 34.192 38.6935C34.2354 38.6447 34.2787 38.5959 34.3221 38.5471C34.6117 38.2193 34.9059 37.8962 35.2038 37.5767C35.2573 37.5196 35.3108 37.4616 35.3643 37.4045C36.0459 36.6818 36.7487 35.9803 37.4728 35.2999C37.5299 35.2465 37.5871 35.194 37.6443 35.1406C37.9653 34.8423 38.2899 34.5487 38.6183 34.2586C38.6672 34.2154 38.7151 34.1721 38.764 34.1298C39.5139 33.4742 40.284 32.8427 41.0735 32.2341C41.14 32.1826 41.2073 32.131 41.2746 32.0804C41.6352 31.8051 41.9996 31.5344 42.3676 31.2693C42.4008 31.2453 42.4331 31.2214 42.4663 31.1975C42.8813 30.9001 43.301 30.6091 43.7262 30.3247C43.7511 30.3081 43.776 30.2924 43.8 30.2759C44.1855 30.019 44.5756 29.7677 44.9695 29.5219C45.0423 29.4767 45.1143 29.4307 45.1871 29.3865C45.604 29.1287 46.0237 28.8755 46.448 28.6307L36.0478 10.6465C25.5267 16.7228 16.7544 25.4792 10.667 35.9812L28.6829 46.3626Z', fill: '#A6DA45' } },
  { rim: { d: 'M70.7164 0V1.38375C58.0842 1.38375 46.2451 4.75521 36.045 10.6465L35.357 9.4561L35.3459 9.43676C45.7461 3.43314 57.8268 0 70.7164 0Z', fill: '#FFFF6B' }, band: { d: 'M47.7632 27.8969C47.8075 27.873 47.8527 27.8499 47.8969 27.8269C48.2779 27.6235 48.6625 27.4264 49.0489 27.2331C49.1319 27.1917 49.215 27.1502 49.2989 27.1097C49.7001 26.9127 50.1032 26.7203 50.5099 26.5334C50.5413 26.5187 50.5726 26.5039 50.604 26.4901C51.0578 26.2839 51.5153 26.0841 51.9764 25.8908C52.0207 25.8724 52.065 25.8549 52.1083 25.8374C52.5077 25.6717 52.9098 25.5115 53.3138 25.3568C53.4134 25.3191 53.513 25.2813 53.6126 25.2436C54.0019 25.0972 54.3929 24.9554 54.7868 24.8191C54.8532 24.7961 54.9196 24.7722 54.9869 24.7492C55.4536 24.5899 55.924 24.4361 56.3972 24.2907C56.4451 24.2759 56.4931 24.2621 56.541 24.2483C56.9524 24.1231 57.3656 24.0034 57.7816 23.8893C57.9006 23.8561 58.0195 23.8248 58.1385 23.7926C58.5139 23.6922 58.8902 23.5965 59.2684 23.5053C59.3689 23.4805 59.4694 23.4556 59.5709 23.4317C60.0468 23.3203 60.5255 23.2144 61.007 23.1168C61.0568 23.1067 61.1066 23.0984 61.1564 23.0883C61.5797 23.0036 62.0059 22.9253 62.4329 22.8517C62.5694 22.8277 62.7068 22.8056 62.8443 22.7836C63.2077 22.7246 63.5729 22.6694 63.9391 22.6178C64.0682 22.5994 64.1973 22.581 64.3265 22.5635C64.8107 22.5 65.2977 22.442 65.7865 22.3923C65.8262 22.3886 65.8658 22.3858 65.9046 22.3821C66.3519 22.338 66.8011 22.3011 67.2512 22.2698C67.3997 22.2597 67.5482 22.2505 67.6976 22.2413C68.0582 22.2192 68.4207 22.2008 68.7832 22.187C68.9298 22.1814 69.0774 22.175 69.225 22.1704C69.7203 22.1557 70.2165 22.1455 70.7155 22.1455V1.38281C58.085 1.38281 46.2441 4.75611 36.045 10.6465L46.4461 28.6288C46.8824 28.3775 47.3205 28.1344 47.7632 27.8969Z', fill: '#EAEA4E' } },
  { rim: { d: 'M106.088 9.43676L106.076 9.4561L105.388 10.6465C95.1883 4.75613 83.3465 1.38375 70.717 1.38375V0C83.6075 0 95.6873 3.43314 106.088 9.43676Z', fill: '#FFD073' }, band: { d: 'M72.2075 22.1686C72.3551 22.1732 72.5017 22.1797 72.6493 22.1852C73.0127 22.199 73.3743 22.2174 73.7349 22.2395C73.8834 22.2487 74.0328 22.257 74.1813 22.268C74.6314 22.2993 75.0806 22.3371 75.5279 22.3804C75.5676 22.384 75.6072 22.3868 75.646 22.3905C76.1348 22.4393 76.6209 22.4982 77.106 22.5617C77.2352 22.5783 77.3643 22.5976 77.4934 22.616C77.8596 22.6676 78.2248 22.7219 78.5882 22.7818C78.7256 22.8039 78.8631 22.826 78.9996 22.8499C79.4275 22.9235 79.8546 23.0018 80.2789 23.0865C80.3277 23.0966 80.3766 23.1049 80.4246 23.1141C80.906 23.2117 81.3847 23.3176 81.8616 23.429C81.963 23.4529 82.0636 23.4787 82.165 23.5026C82.5432 23.5938 82.9195 23.6895 83.2949 23.7899C83.4139 23.8221 83.5338 23.8534 83.6527 23.8866C84.0678 24.0007 84.4819 24.1204 84.8924 24.2456C84.9403 24.2603 84.9892 24.2742 85.0372 24.2889C85.5103 24.4343 85.9798 24.5881 86.4474 24.7474C86.5147 24.7704 86.5811 24.7943 86.6485 24.8183C87.0423 24.9545 87.4325 25.0963 87.8217 25.2427C87.9213 25.2804 88.0218 25.3182 88.1214 25.3569C88.5263 25.5124 88.9285 25.6717 89.3279 25.8374C89.3712 25.8558 89.4155 25.8724 89.4588 25.8908C89.92 26.0842 90.3784 26.284 90.8322 26.4902C90.8635 26.504 90.894 26.5187 90.9244 26.5335C91.3312 26.7194 91.7351 26.9128 92.1364 27.1098C92.2203 27.1512 92.3033 27.1926 92.3863 27.2341C92.7728 27.4274 93.1565 27.6244 93.5374 27.827C93.5817 27.8509 93.6269 27.8739 93.672 27.8979C94.1148 28.1354 94.5529 28.3794 94.9873 28.6298L105.388 10.6475C95.1893 4.75709 83.3484 1.38379 70.7179 1.38379V22.1456C71.216 22.1437 71.7122 22.1539 72.2075 22.1686Z', fill: '#FFAA00' } },
  { rim: { d: 'M131.981 35.2804L131.959 35.2942L130.767 35.981C124.679 25.4781 115.908 16.7226 105.389 10.6463L106.077 9.45586L106.088 9.43652C116.826 15.6289 125.775 24.562 131.981 35.2804Z', fill: '#FF935E' }, band: { d: 'M94.9863 28.6287C95.4106 28.8736 95.8303 29.1268 96.2462 29.3846C96.3191 29.4297 96.392 29.4757 96.4648 29.5208C96.8577 29.7666 97.247 30.0171 97.6325 30.2739C97.6574 30.2905 97.6823 30.3071 97.7072 30.3236C98.1315 30.6081 98.5521 30.8991 98.9671 31.1955C99.0003 31.2194 99.0326 31.2434 99.0658 31.2673C99.4338 31.5325 99.7981 31.8031 100.159 32.0784C100.226 32.13 100.293 32.1815 100.361 32.2331C101.15 32.8416 101.92 33.4732 102.669 34.1287C102.718 34.1711 102.766 34.2143 102.815 34.2576C103.143 34.5467 103.468 34.8413 103.788 35.1387C103.845 35.1921 103.903 35.2455 103.961 35.2989C104.685 35.9783 105.387 36.6808 106.069 37.4035C106.123 37.4606 106.176 37.5186 106.23 37.5757C106.528 37.8951 106.823 38.2192 107.112 38.547C107.156 38.5958 107.199 38.6436 107.241 38.6924C107.897 39.4409 108.531 40.2097 109.14 40.9969C109.192 41.0641 109.244 41.1313 109.295 41.1985C109.571 41.5585 109.842 41.9221 110.108 42.2895C110.132 42.3226 110.156 42.3548 110.18 42.388C110.478 42.8023 110.769 43.2212 111.053 43.6456C111.07 43.6704 111.087 43.6953 111.103 43.7202C111.361 44.105 111.611 44.4935 111.858 44.8857C111.904 44.9585 111.949 45.0312 111.994 45.1039C112.252 45.5191 112.506 45.938 112.751 46.3615L130.766 35.9793C124.679 25.4773 115.907 16.7209 105.386 10.6445L94.9863 28.6287Z', fill: '#DF632D' } },
  { rim: { d: 'M141.435 70.5894H140.049C140.049 57.9801 136.669 46.1625 130.767 35.9809L131.959 35.2941L131.981 35.2803C137.997 45.6644 141.435 57.7195 141.435 70.5894Z', fill: '#FF6E6E' }, band: { d: 'M112.752 46.3628C113.003 46.7964 113.247 47.2338 113.485 47.6757C113.509 47.7199 113.532 47.765 113.556 47.8101C113.759 48.1903 113.958 48.5733 114.15 48.9591C114.192 49.0419 114.233 49.1257 114.275 49.2086C114.472 49.6091 114.665 50.0114 114.852 50.4183C114.866 50.4496 114.881 50.48 114.896 50.5104C115.103 50.9633 115.303 51.4209 115.496 51.8812C115.515 51.9245 115.531 51.9687 115.55 52.012C115.716 52.4106 115.876 52.812 116.031 53.2162C116.07 53.3156 116.108 53.4151 116.145 53.5154C116.291 53.903 116.432 54.2934 116.57 54.6865C116.593 54.7537 116.618 54.82 116.641 54.8872C116.8 55.3531 116.954 55.8226 117.1 56.2949C117.115 56.3428 117.129 56.3916 117.143 56.4394C117.269 56.85 117.388 57.2625 117.503 57.6768C117.536 57.7956 117.568 57.9143 117.6 58.034C117.701 58.4078 117.796 58.7834 117.888 59.1618C117.913 59.2631 117.938 59.3634 117.962 59.4647C118.073 59.9398 118.179 60.4176 118.277 60.8991C118.287 60.9479 118.295 60.9967 118.305 61.0446C118.389 61.4681 118.468 61.8943 118.542 62.3215C118.566 62.4578 118.588 62.595 118.61 62.7321C118.669 63.0949 118.724 63.4595 118.776 63.825C118.794 63.9539 118.813 64.0827 118.83 64.2116C118.894 64.695 118.952 65.1811 119.002 65.669C119.006 65.7086 119.008 65.7482 119.012 65.7869C119.056 66.2334 119.093 66.6818 119.125 67.1311C119.135 67.2793 119.144 67.4275 119.153 67.5767C119.175 67.9366 119.194 68.2985 119.208 68.6603C119.213 68.8067 119.22 68.954 119.224 69.1013C119.239 69.5957 119.249 70.091 119.249 70.5891H140.049C140.049 57.9815 136.669 46.1621 130.768 35.9814L112.752 46.3628Z', fill: '#D83123' } },
]
export const ring = { d: 'M119.224 69.0989C119.219 68.9516 119.213 68.8053 119.207 68.6579C119.193 68.2961 119.175 67.9343 119.153 67.5743C119.144 67.4261 119.135 67.277 119.124 67.1287C119.093 66.6794 119.056 66.2311 119.012 65.7846C119.008 65.745 119.005 65.7054 119.002 65.6667C118.953 65.1788 118.894 64.6936 118.83 64.2093C118.813 64.0804 118.794 63.9515 118.776 63.8226C118.724 63.4571 118.67 63.0926 118.61 62.7298C118.587 62.5926 118.565 62.4555 118.541 62.3192C118.468 61.892 118.389 61.4657 118.304 61.0422C118.294 60.9935 118.286 60.9447 118.277 60.8968C118.179 60.4162 118.073 59.9384 117.961 59.4624C117.937 59.3611 117.911 59.2608 117.887 59.1595C117.796 58.782 117.7 58.4064 117.6 58.0317C117.567 57.9129 117.536 57.7932 117.503 57.6745C117.388 57.2602 117.269 56.8468 117.143 56.4371C117.128 56.3892 117.115 56.3404 117.1 56.2926C116.954 55.8203 116.801 55.3516 116.64 54.8849C116.617 54.8177 116.593 54.7514 116.569 54.6842C116.433 54.291 116.291 53.9016 116.144 53.5131C116.106 53.4137 116.069 53.3133 116.031 53.2139C115.875 52.8097 115.715 52.4083 115.549 52.0097C115.531 51.9664 115.514 51.9222 115.496 51.8789C115.302 51.4186 115.102 50.961 114.895 50.5081C114.882 50.4768 114.867 50.4464 114.852 50.416C114.666 50.01 114.472 49.6067 114.275 49.2062C114.233 49.1225 114.192 49.0396 114.15 48.9567C113.956 48.571 113.759 48.188 113.556 47.8078C113.532 47.7636 113.509 47.7185 113.485 47.6733C113.247 47.2314 113.003 46.7941 112.752 46.3605C112.507 45.937 112.253 45.5181 111.995 45.1029C111.949 45.0301 111.903 44.9574 111.858 44.8847C111.612 44.4925 111.361 44.1039 111.104 43.7191C111.087 43.6943 111.07 43.6694 111.054 43.6445C110.769 43.221 110.477 42.8012 110.18 42.3869C110.156 42.3538 110.132 42.3215 110.108 42.2884C109.843 41.9211 109.572 41.5574 109.296 41.1974C109.244 41.1302 109.193 41.063 109.141 40.9958C108.531 40.2077 107.899 39.439 107.242 38.6914C107.199 38.6426 107.156 38.5947 107.113 38.5459C106.823 38.2182 106.529 37.8941 106.23 37.5746C106.177 37.5175 106.123 37.4595 106.07 37.4025C105.388 36.6797 104.685 35.9782 103.961 35.2978C103.904 35.2444 103.846 35.191 103.789 35.1376C103.469 34.8393 103.144 34.5457 102.816 34.2566C102.767 34.2133 102.719 34.17 102.67 34.1277C101.92 33.4731 101.15 32.8406 100.361 32.232C100.294 32.1805 100.227 32.1289 100.159 32.0774C99.7986 31.8021 99.4343 31.5314 99.0663 31.2663C99.0331 31.2423 99.0008 31.2184 98.9676 31.1945C98.5526 30.8971 98.1329 30.6062 97.7077 30.3226C97.6828 30.306 97.6579 30.2894 97.633 30.2729C97.2475 30.016 96.8583 29.7656 96.4654 29.5198C96.3925 29.4737 96.3196 29.4286 96.2468 29.3835C95.8308 29.1257 95.4111 28.8725 94.9868 28.6277C94.5524 28.3772 94.1143 28.1333 93.6716 27.8957C93.6273 27.8718 93.5821 27.8488 93.5369 27.8248C93.156 27.6223 92.7723 27.4244 92.3859 27.2319C92.3029 27.1905 92.2189 27.1491 92.1359 27.1076C91.7347 26.9106 91.3307 26.7182 90.924 26.5313C90.8926 26.5175 90.8622 26.5028 90.8317 26.488C90.3779 26.2809 89.9196 26.0811 89.4584 25.8887C89.415 25.8703 89.3708 25.8537 89.3274 25.8353C88.928 25.6696 88.5259 25.5094 88.121 25.3547C88.0214 25.316 87.9218 25.2783 87.8212 25.2405C87.4329 25.0942 87.0419 24.9533 86.648 24.8161C86.5807 24.7931 86.5143 24.7682 86.447 24.7452C85.9803 24.586 85.5099 24.4322 85.0367 24.2867C84.9888 24.272 84.9399 24.2582 84.8919 24.2435C84.4806 24.1183 84.0673 23.9995 83.6523 23.8844C83.5333 23.8513 83.4143 23.82 83.2944 23.7877C82.92 23.6874 82.5437 23.5916 82.1646 23.5005C82.0631 23.4756 81.9626 23.4508 81.8611 23.4268C81.3852 23.3154 80.9065 23.2096 80.4241 23.112C80.3752 23.1019 80.3264 23.0936 80.2784 23.0844C79.8541 22.9997 79.4271 22.9214 78.9991 22.8478C78.8626 22.8247 78.7252 22.8017 78.5878 22.7796C78.2244 22.7207 77.8591 22.6655 77.493 22.6139C77.3638 22.5955 77.2347 22.5771 77.1056 22.5596C76.6214 22.4961 76.1344 22.4381 75.6455 22.3883C75.6059 22.3847 75.5662 22.3819 75.5275 22.3782C75.0801 22.334 74.631 22.2972 74.1809 22.2659C74.0324 22.2558 73.8839 22.2466 73.7344 22.2374C73.3738 22.2153 73.0113 22.1968 72.6489 22.183C72.5022 22.1775 72.3546 22.1711 72.2071 22.1665C71.7118 22.1517 71.2156 22.1416 70.7166 22.1416C70.2176 22.1416 69.7214 22.1517 69.2261 22.1665C69.0785 22.1711 68.9319 22.1775 68.7843 22.183C68.4218 22.1968 68.0593 22.2153 67.6987 22.2374C67.5502 22.2466 67.4008 22.2548 67.2523 22.2659C66.8022 22.2972 66.353 22.334 65.9057 22.3782C65.866 22.3819 65.8264 22.3847 65.7876 22.3883C65.2988 22.4381 64.8127 22.4961 64.3276 22.5596C64.1984 22.5762 64.0693 22.5955 63.9402 22.6139C63.574 22.6655 63.2088 22.7198 62.8454 22.7796C62.708 22.8017 62.5705 22.8238 62.434 22.8478C62.0061 22.9214 61.5809 22.9997 61.1575 23.0844C61.1077 23.0945 61.0579 23.1028 61.0081 23.1129C60.5266 23.2105 60.048 23.3164 59.572 23.4278C59.4706 23.4517 59.37 23.4775 59.2695 23.5014C58.8913 23.5926 58.5141 23.6883 58.1396 23.7887C58.0207 23.8209 57.9017 23.8522 57.7827 23.8853C57.3667 23.9995 56.9535 24.1192 56.5422 24.2444C56.4942 24.2591 56.4462 24.272 56.3983 24.2867C55.9251 24.4322 55.4547 24.586 54.988 24.7452C54.9207 24.7682 54.8552 24.7922 54.7879 24.8152C54.3941 24.9515 54.003 25.0932 53.6138 25.2396C53.5142 25.2774 53.4145 25.3151 53.3149 25.3529C52.91 25.5085 52.5079 25.6677 52.1094 25.8335C52.0652 25.8519 52.0209 25.8694 51.9775 25.8869C51.5164 26.0802 51.0589 26.28 50.6051 26.4862C50.5738 26.5 50.5424 26.5157 50.511 26.5295C50.1043 26.7154 49.7003 26.9079 49.3 27.1058C49.217 27.1472 49.1331 27.1877 49.0501 27.2292C48.6627 27.4225 48.279 27.6205 47.8981 27.823C47.8538 27.8469 47.8086 27.869 47.7643 27.893C47.3216 28.1305 46.8835 28.3745 46.4491 28.6249C46.0239 28.8698 45.6051 29.123 45.1883 29.3808C45.1154 29.4259 45.0425 29.471 44.9706 29.5161C44.5777 29.7619 44.1875 30.0132 43.8011 30.2701C43.7762 30.2867 43.7513 30.3023 43.7273 30.3189C43.3021 30.6034 42.8824 30.8943 42.4674 31.1917C42.4342 31.2156 42.4019 31.2396 42.3687 31.2635C41.9997 31.5287 41.6364 31.7993 41.2757 32.0746C41.2084 32.1262 41.1411 32.1768 41.0746 32.2284C40.2851 32.8369 39.515 33.4685 38.7651 34.124C38.7162 34.1663 38.6683 34.2096 38.6194 34.2529C38.291 34.5429 37.9664 34.8366 37.6454 35.1349C37.5882 35.1883 37.531 35.2408 37.4739 35.2942C36.7498 35.9745 36.0461 36.6761 35.3654 37.3988C35.3119 37.4559 35.2584 37.5139 35.2049 37.5709C34.907 37.8904 34.6128 38.2145 34.3232 38.5413C34.2798 38.5901 34.2365 38.6389 34.1931 38.6877C33.5373 39.4353 32.9046 40.204 32.295 40.9921C32.2433 41.0593 32.1917 41.1265 32.14 41.1937C31.8652 41.5537 31.594 41.9165 31.3284 42.2838C31.3044 42.3169 31.2795 42.3501 31.2555 42.3842C30.9576 42.7985 30.667 43.2173 30.382 43.6409C30.3654 43.6657 30.3488 43.6906 30.3322 43.7154C30.0749 44.1003 29.824 44.4888 29.5778 44.881C29.5317 44.9537 29.4865 45.0264 29.4413 45.0992C29.183 45.5144 28.9294 45.9333 28.684 46.3568C28.4332 46.7904 28.1887 47.2277 27.9508 47.6697C27.9268 47.7139 27.9037 47.759 27.8798 47.8041C27.6768 48.1843 27.4785 48.5673 27.2858 48.9531C27.2443 49.0359 27.2028 49.1197 27.1613 49.2035C26.9639 49.603 26.7711 50.0054 26.5848 50.4114C26.57 50.4427 26.5553 50.474 26.5405 50.5062C26.3339 50.9592 26.1338 51.4158 25.9401 51.8762C25.9216 51.9203 25.9041 51.9645 25.8857 52.0087C25.7196 52.4065 25.5601 52.8079 25.4042 53.2111C25.3655 53.3105 25.3277 53.4109 25.2898 53.5103C25.1432 53.8979 25.0021 54.2883 24.8646 54.6814C24.8416 54.7486 24.8167 54.8149 24.7936 54.8821C24.6341 55.348 24.48 55.8175 24.3343 56.2898C24.3195 56.3377 24.3057 56.3865 24.291 56.4343C24.1655 56.844 24.0465 57.2574 23.9312 57.6717C23.898 57.7905 23.8667 57.9092 23.8344 58.0289C23.7339 58.4027 23.6379 58.7793 23.5466 59.1567C23.5217 59.258 23.4968 59.3584 23.4728 59.4596C23.3612 59.9347 23.2552 60.4125 23.1574 60.8931C23.1473 60.9428 23.139 60.9925 23.1288 61.0422C23.044 61.4648 22.9656 61.8902 22.8918 62.3164C22.8678 62.4527 22.8457 62.5899 22.8235 62.7271C22.7645 63.0898 22.7092 63.4544 22.6575 63.8199C22.6391 63.9488 22.6206 64.0777 22.6031 64.2066C22.5394 64.6899 22.4813 65.176 22.4315 65.664C22.4278 65.7035 22.4251 65.7431 22.4214 65.7818C22.3771 66.2283 22.3402 66.6767 22.3089 67.126C22.2987 67.2742 22.2895 67.4224 22.2803 67.5716C22.2581 67.9315 22.2397 68.2934 22.2259 68.6552C22.2203 68.8016 22.2139 68.9489 22.2093 69.0962C22.1945 69.5906 22.1843 70.0859 22.1843 70.584H29.1175C29.1175 63.0189 31.1448 55.928 34.6856 49.8194C38.3381 43.5184 43.6018 38.2642 49.9143 34.6184C56.034 31.084 63.1378 29.0604 70.7166 29.0604C78.2954 29.0604 85.3992 31.084 91.5189 34.6184C97.8313 38.2642 103.095 43.5184 106.748 49.8194C110.288 55.928 112.316 63.0189 112.316 70.584H119.249C119.249 70.0896 119.239 69.5933 119.224 69.0989Z', fill: 'black' }
export const face = { d: 'M106.747 49.8245C103.095 43.5235 97.831 38.2693 91.5185 34.6235C85.3989 31.089 78.2951 29.0654 70.7162 29.0654C63.1374 29.0654 56.0336 31.089 49.9139 34.6235C43.6015 38.2693 38.3377 43.5235 34.6853 49.8245C31.1445 55.9331 29.1172 63.024 29.1172 70.589H70.7153H112.315C112.315 63.024 110.288 55.9331 106.747 49.8245Z', fill: '#212121' }
export const needle = { d: 'M111.117 36.1662L74.1669 59.8714C73.0943 59.4644 71.9321 59.2389 70.7165 59.2389C65.3559 59.2389 61.0099 63.577 61.0099 68.9279C61.0099 74.2788 65.3559 78.6169 70.7165 78.6169C76.0771 78.6169 80.4231 74.2788 80.4231 68.9279C80.4231 68.2494 80.3521 67.5865 80.2193 66.9466L111.134 36.1865C111.146 36.1754 111.131 36.1579 111.117 36.1662Z', fill: 'white' }
```

## Source — shared app parts it uses

### `src/shared/copy.ts`

```ts
/**
 * UI copy. Every user-facing string of a flow lives in ONE file, `src/flows/<flow-id>/copy.json` — flat keys
 * ("empty.title") → the text. Screens read it through the flow's `t`:
 *
 *   t('empty.title')                       → "No search results found"
 *   t('group.results', { list, count })    → "Stocks (8)"   ({name} placeholders are filled in)
 *
 * Sample data (stock names, prices, watchlists) stays in the flow's data.ts — copy.json is only the words the app
 * itself says: titles, labels, buttons, empty states, accessible names. Porting a flow: this file is the string
 * table (Flutter ARB / iOS Localizable.strings).
 */
export type CopyVars = Record<string, string | number>

export function makeCopy<Copy extends Record<string, string>>(flow: string, copy: Copy) {
  return function t(key: keyof Copy & string, vars?: CopyVars): string {
    const template = copy[key] ?? key
    return vars ? template.replace(/\{(\w+)\}/g, (m, name: string) => (name in vars ? String(vars[name]) : m)) : template
  }
}
```

### `src/shared/data/format.ts`

```ts
/** ₹ with Indian grouping: inr(123456.5) → "₹1,23,456.50"; inr(1000, 0) → "₹1,000". */
export const inr = (n: number, decimals = 2) =>
  '₹' + n.toLocaleString('en-IN', { minimumFractionDigits: decimals, maximumFractionDigits: decimals })

/** $ with US grouping: usd(1234.5) → "$1,234.50"; usd(1000, 0) → "$1,000". */
export const usd = (n: number, decimals = 2) =>
  '$' + n.toLocaleString('en-US', { minimumFractionDigits: decimals, maximumFractionDigits: decimals })

/** "9876543210" → "98765 43210" (how Indian mobile numbers are read out). */
export const phone = (digits: string) => (digits.length > 5 ? `${digits.slice(0, 5)} ${digits.slice(5)}` : digits)

/** Keep the last `visible` characters: mask('ABCPM1234K', 1, 5) → "ABCPM****K". */
export const mask = (s: string, keepStart: number, keepEnd: number) =>
  s.slice(0, keepStart) + '•'.repeat(Math.max(0, s.length - keepStart - keepEnd)) + s.slice(s.length - keepEnd)
```

### `src/shared/prototype.ts`

```ts
// Prototype behaviour the flows use — the ONLY way a flow touches the preview (src/preview). In a real app: interactive
// is always true, `go` is the app's navigation, and useAutoAdvance is "when the server answers, go on" (a loader /
// polling step).
import { createContext, useContext, useEffect, useSyncExternalStore } from 'react'

export type Preview = {
  /**
   * Prototype (true): real modal L3 BottomSheets, taps move to the next screen, loaders advance on their own.
   * Board (false): every screen frozen as in the Figma board — sheets are drawn as static surfaces.
   */
  interactive: boolean
  /** Go to another screen of the flow (by id). Does nothing on the board. */
  go: (id: string) => void
  /** The screen being shown — a new id gives the sheet a fresh mount while the page behind it stays put. */
  screenId?: string
  /** Back to the screen shown before this one in the prototype (any flow); false when there's none to go back to. */
  back?: () => boolean
  /** The screen was reached by going back (a back button, or a screen earlier in the app) — a page can come back as it was left. */
  cameBack?: boolean
  /** An `over` screen (a sheet over the screen the user is on): the screen under it — closing the sheet goes there. */
  under?: string
}

export const PreviewContext = createContext<Preview>({ interactive: false, go: () => {} })
export const usePreview = () => useContext(PreviewContext)

// ---- Which way the next move goes ------------------------------------------------------------------------------------
// The preview guesses a move's direction (a screen visited before, or one earlier in the same flow, is "back"). A tap can
// say it instead: Deposit opens Add money (listed earlier in the flow) as a page going forward; a back button's fallback
// (nothing in the history) is back, and replaces the page instead of adding to the history.
let intent: { id: string; direction: 'forward' | 'back' } | null = null

/** Say the move to screen `id` that is about to happen is forward (a page opening) or back. */
export const markMove = (id: string, direction: 'forward' | 'back') => {
  intent = { id, direction }
}

/** The preview: the direction said for a move to `id`, if any. */
export const moveIntent = (id: string) => (intent?.id === id ? intent.direction : undefined)

/** The preview: the move happened. */
export const clearMoveIntent = () => {
  intent = null
}

/** Prototype only: after `ms`, go to `next` (loaders, "Verifying…"). */
export function useAutoAdvance(next: string | undefined, ms: number) {
  const { interactive, go } = usePreview()
  useEffect(() => {
    if (!interactive || !next) return
    const t = window.setTimeout(() => go(next), ms)
    return () => window.clearTimeout(t)
  }, [interactive, next, ms, go])
}

/** How long a loader or "Verifying…" state stays up in the prototype. */
export const LOADING_MS = 1600

// ---- Flow options (prototype settings such as a design version) ----------------------------------------------------
// Picked in the preview (desktop panel, or the settings sheet on a phone), remembered per browser. A real app has no
// such switch: it ships one version.

// The key carries the option's default: when a flow changes its default (e.g. to a new version), earlier picks reset.
const optionKey = (flow: string, option: string, fallback: string) => `prototype-option:${flow}.${option}@${fallback}`
const optionListeners = new Set<() => void>()

function readOption(key: string): string | null {
  try { return localStorage.getItem(key) } catch { return null }
}

/** The preview sets an option (and every screen reading it re-renders). */
export function setFlowOption(flow: string, option: string, fallback: string, value: string) {
  const key = optionKey(flow, option, fallback)
  try { localStorage.setItem(key, value) } catch { /* storage off: this visit only */ }
  memory.set(key, value)
  optionListeners.forEach((l) => l())
}

const memory = new Map<string, string>()

/**
 * Showcase links (20 Nov): `?showcase` shows one screen as it is shared (e.g. the risk-meter page for leadership) — no
 * prototype controls over it. `?o=nudges.meter:soft,nudges.meterFill:split` sets options for this page only (never
 * stored, so several showcase frames side by side can each show their own version).
 */
// A showcase frame can't always carry a query (a packed, single-file page loads the app from a blob: URL), so its
// settings can also come in the frame's name: "showcase:?showcase&theme=dark&o=…".
export const linkParams = (() => {
  try {
    const named = window.name.startsWith('showcase:') ? window.name.slice('showcase:'.length) : ''
    return new URLSearchParams(named || location.search)
  } catch { return new URLSearchParams() }
})()
export const SHOWCASE = linkParams.has('showcase')
// A showcase frame sits in a page of phones: the app focusing its sheet must never scroll that page to it.
if (SHOWCASE) {
  const focus = HTMLElement.prototype.focus
  HTMLElement.prototype.focus = function (options?: FocusOptions) { focus.call(this, { ...options, preventScroll: true }) }
}
const linkOptions = new Map((linkParams.get('o') ?? '').split(',').filter(Boolean).map((pair) => {
  const [key, value = ''] = pair.split(':')
  return [key, value] as const
}))
const picked = (flow: string, option: string, fallback: string) => {
  const key = optionKey(flow, option, fallback)
  return linkOptions.get(`${flow}.${option}`) ?? memory.get(key) ?? readOption(key)
}

/** A flow's prototype option: the picked value, or `fallback` (the option's default) when none is picked. */
export function useFlowOption(flow: string, option: string, fallback: string) {
  return useSyncExternalStore(
    (onChange) => { optionListeners.add(onChange); return () => { optionListeners.delete(onChange) } },
    () => picked(flow, option, fallback) ?? fallback,
  )
}

type OptionLike = { id: string; default: string; showIf?: Record<string, string[]> }

/** Every option of a flow as it's set now (the settings panel's show / hide and nesting). */
export function useFlowOptionValues(flow: { id: string; options?: OptionLike[] }): Record<string, string> {
  const snap = useSyncExternalStore(
    (onChange) => { optionListeners.add(onChange); return () => { optionListeners.delete(onChange) } },
    () => JSON.stringify(Object.fromEntries((flow.options ?? []).map((o) => {
      return [o.id, picked(flow.id, o.id, o.default) ?? o.default]
    }))),
  )
  return JSON.parse(snap)
}

/** Whether an option matters right now (every option it depends on has one of the listed values, all the way up). */
export function optionShown(options: OptionLike[], option: OptionLike, values: Record<string, string>): boolean {
  return Object.entries(option.showIf ?? {}).every(([id, allowed]) => {
    const parent = options.find((o) => o.id === id)
    return allowed.includes(values[id]) && (!parent || optionShown(options, parent, values))
  })
}

/** How deep an option sits under the ones it depends on (0 = top level) — the panel indents it that much. */
export function optionDepth(options: OptionLike[], option: OptionLike): number {
  const parents = Object.keys(option.showIf ?? {}).map((id) => options.find((o) => o.id === id)).filter(Boolean) as OptionLike[]
  return parents.length ? 1 + Math.max(...parents.map((p) => optionDepth(options, p))) : 0
}

/** How many of a flow's options are off their default (the settings panel's badge). */
export function useFlowOptionsChanged(flow: { id: string; options?: { id: string; default: string }[] }) {
  return useSyncExternalStore(
    (onChange) => { optionListeners.add(onChange); return () => { optionListeners.delete(onChange) } },
    () => (flow.options ?? []).filter((o) => {
      return (picked(flow.id, o.id, o.default) ?? o.default) !== o.default
    }).length,
  )
}

/**
 * Typing on a phone uses the phone's own keyboard (a touch screen, `pointer: coarse`) — the prototype's drawn keyboard
 * stays hidden. On a laptop the drawn keyboard shows (for the look) and the laptop's keys type into the focused field.
 */
export function useSystemKeyboard() {
  return typeof window !== 'undefined' && window.matchMedia('(pointer: coarse)').matches
}
```

### `src/shared/ui/Screen.module.css`

```css
/*
 * One phone screen: the OS status bar, then the page — top bar, a body that scrolls and fills the space, the bottom
 * bar / button dock — with an optional toast just above the bottom and a sheet over everything.
 */
.screen {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: clip; /* clip, not hidden: a sheet taking focus while it slides in can't scroll the screen */
  background: var(--l3-surface-default);
  color: var(--l3-content-primary);
  /* A size container: a sheet can be a share of the phone screen's height (cqh), in the mockup and on a phone alike. */
  container: app-screen / size;
  /* An app, not a page: swiping and long-pressing never select text (fields still do — see below). */
  user-select: none;
  -webkit-user-select: none;
  -webkit-touch-callout: none;
}

.screen :is(input, textarea, [contenteditable='true']) {
  user-select: text;
  -webkit-user-select: text;
}

.page {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
}

.middle {
  position: relative;
  display: flex;
  flex: 1;
  min-height: 0;
}

/* Like a phone: no scrollbar that stays on screen — the page, rails and open sheets still scroll (touch, wheel, keys). */
.screen,
.screen * {
  scrollbar-width: none;
}

.screen ::-webkit-scrollbar {
  display: none;
}

.body {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
  overflow-x: hidden; /* a screen never scrolls sideways — only rails inside it do */
  overflow-y: auto;
  padding-bottom: var(--app-body-end);
}

/* Bottom navbar V2 (floating, shared/ui/FloatingNav): the body runs on behind the pill; its end clears the pill
   (14 up + 56 tall) plus the usual 24. */
.page:has(> [data-floating-nav]) .body {
  padding-bottom: calc(var(--app-body-end) + var(--l3-spacing-14) + var(--l3-size-56));
}

/* The body owns the page's end spacing: whatever comes last adds none of its own (no 24 + 24). */
.body > :last-child {
  padding-bottom: 0;
  margin-bottom: 0;
}
/* A page's last block nested deeper (inside tabs or a panel) is marked data-page-end so it adds none either. */
.body [data-page-end] { padding-bottom: 0; margin-bottom: 0; }

/* Floating toast: sits on the bottom edge of the body, above the dock / navbar (L3 Aerobar rule). */
.toast {
  position: absolute;
  inset: auto 0 0;
}

/* The prototype's modal L3 sheet is portalled into this screen — lift it above the bottom bars. */
.screen.screen > [data-contained] {
  z-index: var(--app-layer-sheet);
}
```

### `src/shared/ui/Screen.tsx`

```tsx
import { Fragment, useState, type ReactNode } from 'react'
import { usePreview } from '../prototype'
import { SheetHost } from './Sheet'
import styles from './Screen.module.css'

type ScreenProps = {
  /** Actionbar (or nothing on full-bleed screens like the welcome screen). */
  top?: ReactNode
  /** Bottom navbar, button dock (ButtonGroup) or the keypad. */
  bottom?: ReactNode
  /** A floating Aerobar toast — shown just above `bottom`. */
  toast?: ReactNode
  /** A Sheet over the screen. */
  sheet?: ReactNode
  /** Screen content: scrolls when taller than the space between `top` and `bottom`. */
  children?: ReactNode
  /** A flow's own styling hook on the screen root (e.g. an experiment scoped to one flow). */
  className?: string
}

/**
 * The frame of every screen: top · body · bottom, a toast above the bottom, and a sheet over it all. It fills the
 * space between the OS status bar and the OS navigation bar (the preview draws those — real apps get them from the
 * OS, edge-to-edge insets), so nothing sits under the gesture handle. The screen is the sheet's container, so sheets
 * and their scrim stay in the app; its SheetHost keeps one sheet open while the screen's sheet changes (no scrim flash
 * between two sheets). Mark a bottom part with data-surface="primary" | "secondary" if the navigation bar should take
 * its colour (the button dock and the bottom navbar are recognised already).
 */
export function Screen({ top, bottom, toast, sheet, children, className }: ScreenProps) {
  const [el, setEl] = useState<HTMLDivElement | null>(null)
  const { screenId } = usePreview()
  return (
    <div ref={setEl} className={[styles.screen, className].filter(Boolean).join(' ')}>
      <SheetHost container={el}>
        <div className={styles.page} data-part="page">
          {top}
          <div className={styles.middle}>
            <div className={styles.body}>{children}</div>
            {toast && <div className={styles.toast}>{toast}</div>}
          </div>
          {bottom}
        </div>
        <Fragment key={screenId}>{sheet}</Fragment>
      </SheetHost>
    </div>
  )
}
```

### `src/shared/ui/Sheet.module.css`

```css
/* Board only: the static sheet sits on the bottom edge of the screen, over the L3 Overlay — shown at rest, no fade-in. */
.scrim {
  z-index: var(--app-layer-scrim);
  transition: none;
}

.dock {
  position: absolute;
  inset: auto 0 0;
  z-index: var(--app-layer-sheet);
  display: flex;
  flex-direction: column;
  max-height: calc(100% - var(--l3-size-48));
}

/* A top sheet (placement="top") hangs from the top edge instead. */
.dock[data-placement='top'] {
  inset: 0 0 auto;
}
```

### `src/shared/ui/Sheet.tsx`

```tsx
import { createContext, useCallback, useContext, useId, useLayoutEffect, useState, type ReactNode } from 'react'
import { BottomSheet, BottomSheetSurface } from '@l3/components/BottomSheet'
import { Overlay } from '@l3/components/Overlay'
import { usePreview } from '../prototype'
import styles from './Sheet.module.css'

/** Lets the L3 sheet play its exit before the next screen replaces it (≈ motion/duration-medium). */
const SHEET_EXIT_MS = 240

type SheetProps = {
  header?: ReactNode
  children?: ReactNode
  /** The button dock — a <ButtonGroup>. */
  footer?: ReactNode
  /** Id of the header's heading (BottomSheetHeader `headingId`). */
  labelledBy?: string
  /** Accessible name when there's no header (loaders). */
  label?: string
  /** Prototype: where a backdrop tap, drag down or Esc goes. Leave out for sheets that can't be dismissed (loaders). */
  onClose?: () => void
  /** A screen's own styling hook on the sheet surface (e.g. a sheet that brings its own layout and no padding). */
  className?: string
  /** 'top' (L3 isBottom=False): drops from the top of the screen, for menus tied to the top (a market or sort picker). */
  placement?: 'bottom' | 'top'
}

type Publish = (id: string, sheet: SheetProps | null) => void
const SheetHostContext = createContext<Publish | null>(null)

/**
 * An L3 bottom sheet over the screen.
 * Prototype: the real modal <BottomSheet> (focus trap, drag to close, scrim tap), drawn by the screen's SheetHost —
 * so when one sheet leads to another over the same page, the scrim stays and only the sheet's content changes.
 * Board: the same surface drawn statically with the L3 Overlay — dozens of open modals on one page would fight
 * over focus, so the board shows <BottomSheetSurface>, which is what the modal renders inside.
 */
export function Sheet(props: SheetProps) {
  const { interactive } = usePreview()
  const publish = useContext(SheetHostContext)
  const id = useId()
  const hosted = interactive && publish !== null

  // Hand the latest content to the host on every render; take it back when this sheet goes away.
  useLayoutEffect(() => { if (hosted) publish(id, props) })
  useLayoutEffect(() => (hosted ? () => publish(id, null) : undefined), [hosted, publish, id])

  if (hosted) return null
  const { header, children, footer, labelledBy, label, className, placement = 'bottom' } = props
  return (
    <>
      <Overlay open className={styles.scrim} />
      <div className={styles.dock} data-placement={placement} role="dialog" aria-labelledby={labelledBy} aria-label={label}>
        <BottomSheetSurface header={header} footer={footer} className={className} placement={placement}>{children}</BottomSheetSurface>
      </div>
    </>
  )
}

/**
 * Prototype: one modal L3 BottomSheet per screen, kept open while the screen's sheet changes (sheet → sheet swaps
 * the content in place, no scrim flash or slide). It closes with its exit animation when the screen has no sheet.
 */
export function SheetHost({ container, children }: { container: HTMLElement | null; children: ReactNode }) {
  const [shown, setShown] = useState<{ id: string; sheet: SheetProps; open: boolean } | null>(null)
  const publish = useCallback<Publish>((id, sheet) => {
    setShown((s) => (sheet ? { id, sheet, open: true } : s && s.id === id ? { ...s, open: false } : s))
  }, [])

  const close = () => {
    const onClose = shown?.sheet.onClose
    if (!onClose) return
    setShown((s) => s && { ...s, open: false })
    window.setTimeout(onClose, SHEET_EXIT_MS)
  }

  return (
    <SheetHostContext.Provider value={publish}>
      {children}
      {container && shown && (
        <BottomSheet
          open={shown.open}
          onClose={close}
          container={container}
          header={shown.sheet.header}
          footer={shown.sheet.footer}
          aria-labelledby={shown.sheet.labelledBy}
          aria-label={shown.sheet.label}
          className={shown.sheet.className}
          placement={shown.sheet.placement}
        >
          {shown.sheet.children}
        </BottomSheet>
      )}
    </SheetHostContext.Provider>
  )
}
```
