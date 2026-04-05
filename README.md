# Bridger: WESTERN Macro

Windows fishing macro for `Bridger: WESTERN`.

It uses:
- loopback audio to detect bite sounds
- a fixed QTE box plus captured templates for `T / R / F / G`
- optional bait use after each catch
- optional movement hold and chest claim handling

## Quick Start

1. Download `Bridger WESTERN Macro.exe`.
2. Put it in its own folder.
3. Run the EXE.

On first run it will create:
- `config.json`
- `fishing_macro\logs\`
- `fishing_macro\samples\`
- `fishing_macro\qte_templates\`

## First-Time Setup

1. Open the game and keep its audio going through your normal Windows playback device.
2. Launch the macro.
3. In the app, pick the correct loopback device and click `Use Selected`.
4. Click `Record Bite Sample`.
5. Wait for a real bite sound, then use `Play Sample` to confirm it recorded the correct sound.
6. Click `Set QTE Box` and drag a tight box over where the QTE prompt appears.
7. Click `Open QTE Trainer` and capture:
   - a few `Blank` frames with no prompt visible
   - a few `T`
   - a few `R`
   - a few `F`
   - a few `G`
8. Turn on `Test mode only (no clicks)` and do a short test run.
9. If bite detection and QTE detection look good, turn test mode off and use `Start / Stop (F8)`.

## What The Macro Does

When running, the macro can:
- cast the rod
- wait for the bite sound
- click to catch
- handle the `T / R / F / G` QTE
- wait for chest claim timing and hold `E`
- use bait
- swap back to the rod
- keep the configured movement keys held
- recast if a bite never happens and the rod likely bugged

## QTE Training

The QTE detector does not use generic OCR.

It works by:
1. capturing only the small fixed box you selected
2. using `Blank` captures to learn what "no prompt" looks like
3. comparing the visible glyph against captured `T / R / F / G` examples
4. only pressing a key when the match is strong enough

For best reliability, capture several clean `Blank`, `T`, `R`, `F`, and `G` frames during setup.

Multiple screenshots per letter help because the prompt can shift slightly, antialiasing changes frame to frame, and some captures are cleaner than others.

## Basic Troubleshooting

- If the macro catches too early or too late, adjust `Trigger Faction` in automation panel.
- If the QTE misses keys, tighten the QTE box and recapture clean `Blank`, `T`, `R`, `F`, and `G` templates.
- If clicks or hotkeys do not work, try running the app as administrator.
