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

- If the bite never triggers, re-record the bite sample and lower `min_bite_delay_after_cast` only if the game really bites that early.
- If the macro catches too early on random sounds, raise `similarity_threshold`, `detection_threshold`, or `energy_threshold`.
- If the QTE misses keys, tighten the QTE box and recapture clean `Blank`, `T`, `R`, `F`, and `G` templates.
- If clicks or hotkeys do not work, try running the app as administrator.

## Advanced `config.json` Reference

The fields below are only for manual tuning.

### Audio Device

- `audio_device_id`: exact loopback device id to open.
- `audio_device_name`: last selected device name.

### Audio Capture

- `sample_rate`: audio capture sample rate.
- `chunk_size`: audio frames processed per chunk.
- `channels`: number of audio channels.
- `ring_buffer_seconds`: how much recent audio to keep in memory.
- `compare_rate_hz`: lower-rate stream used for envelope matching.
- `waveform_compare_rate_hz`: higher-rate stream used for waveform comparison.

### Bite Timing

- `cast_delay`: wait after casting before entering the listening phase.
- `min_bite_delay_after_cast`: earliest time after cast that a bite is allowed to trigger.
- `bite_wait_timeout_seconds`: max wait before forcing a recast.
- `catch_cooldown`: cooldown after catch before the next cycle.
- `refractory_period`: ignore repeated bite matches for this long after a trigger.
- `min_time_between_clicks`: hard click safety gap.
- `click_hold_seconds`: how long a click is held down.

### Bite Matching

- `detection_threshold`: final combined confidence threshold.
- `similarity_threshold`: similarity score required for the bite sample.
- `energy_threshold`: minimum audio energy before matching is considered.
- `consecutive_matches_required`: how many matching chunks are needed in a row.
- `score_smoothing_window`: rolling smoothing size for confidence.
- `search_margin_seconds`: extra live audio search window around the sample.
- `trigger_sample_fraction`: fraction of the bite sample used for early trigger matching.

### Bite Sample Recording

- `sample_record_seconds`: total recorded sample length.
- `sample_pre_roll_seconds`: audio saved from just before the trigger.
- `sample_record_trigger_level`: loudness needed to start recording a new sample.
- `last_sample_path`: last loaded or saved bite sample path.

### Debug

- `debug_clip_seconds`: debug clip length around triggers.
- `save_debug_clips`: save trigger-area clips into `fishing_macro\logs\`.
- `debug_mode`: extra debug behavior flag.
- `debug_log_file`: write logs to file.
- `test_mode`: detect and log, but do not send real clicks or keys.

### General UI / Behavior

- `arm_after_cast_only`: only allow bite detection during a real cast cycle.
- `always_on_top`: keep the window floating.

### Bait Automation

- `bait_enabled`: enable bait use after catch.
- `bait_slot_key`: hotbar key for bait.
- `rod_slot_key`: hotbar key for rod.
- `bait_click_count`: how many bait clicks to send.
- `bait_equip_delay`: delay after switching to bait.
- `bait_click_interval`: spacing between bait clicks.
- `bait_return_delay`: delay before switching back to the rod.

### Movement Hold

- `movement_hold_enabled`: keep movement keys held while running.
- `movement_hold_keys`: keys to hold, comma-separated, for example `s,d`.
- `movement_rehold_pulse_seconds`: release/repress pulse after interactions to restore movement.

### QTE Detection

- `qte_enabled`: enable QTE scanning.
- `qte_scan_interval`: time between QTE scans.
- `qte_match_threshold`: minimum match score for a key.
- `qte_blank_margin`: how much better a key must look than a blank frame.
- `qte_score_margin`: how much the best key must beat the second-best key.
- `qte_consecutive_hits_required`: same key must be seen this many scans in a row.
- `qte_prompt_cooldown`: minimum time between QTE key presses.
- `qte_allowed_keys`: allowed prompt keys, currently `TRFG`.

### QTE Flow / Chest Claim

- `qte_claim_hold_enabled`: enable automatic chest claim hold.
- `qte_claim_hold_key`: claim key.
- `qte_claim_start_delay_seconds`: wait after QTE completion before starting claim.
- `qte_claim_hold_seconds`: how long to hold the claim key.
- `qte_post_catch_wait_seconds`: how long to wait after a catch for a QTE to begin.
- `qte_resume_quiet_seconds`: how long the QTE must stay quiet before fishing resumes.

### QTE Box / Window Matching

- `qte_roi_left`: left edge of the saved QTE box, relative to the game window.
- `qte_roi_top`: top edge of the saved QTE box, relative to the game window.
- `qte_roi_right`: right edge of the saved QTE box, relative to the game window.
- `qte_roi_bottom`: bottom edge of the saved QTE box, relative to the game window.
- `qte_window_title_hint`: window-title matcher used to find the game window.

### Hotkeys

- `hotkey_start_stop`: default start/stop hotkey, usually `f8`.
- `hotkey_quit`: default quit hotkey, usually `f9`.
- `hotkey_manual_cast`: hidden helper hotkey for a one-off cast.
- `hotkey_manual_arm`: hidden helper hotkey for a one-off detector arm.
