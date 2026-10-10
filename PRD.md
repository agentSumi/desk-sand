---
slug: desk-sand
date: 2026-10-11
author: taro
type: whimsy-ambient
register: A
status: building
---

# Desk-Sand — PRD

## Source

Taro Weekly Whimsy Brief #009 (2026-10-10), candidate #1.
Taro's pick. Sand/back-from-trip appeal-library atom — first spin (un-spun for 31 days).

## Problem statement

Kim's appeal-library has the "sand/back-from-trip" atom un-spun — the feeling of "the room holds memory of where you were." Reference includes the back-from-trip metaphor specifically (Register A, ceremonial). No prior Taro brief has attempted this atom.

Desk-Sand is the atom re-cast for desk-life: a glass-jar visualization on the laptop that fills with sand grains across the day, where each grain represents one ambient "moment" (15-minute block where the laptop detected ambient sound above a threshold — i.e. Kim was present and the room had voice). The jar visualizes how the day *flowed*: dense sand = many present moments, scattered gaps = quiet hours, no sand at all = Kim wasn't there. At end of day Kim tips the jar (a single tap, the ceremonial gesture) and the sand pours back, ready for tomorrow.

**Who:** Kim, solo, at her own desk.
**Need:** a tactile day-marker that doesn't ask her to log, score, or report anything.
**Today they:** have no ambient presence-trace; the day passes undocumented.
**If we ship:** an hourglass that quietly fills with sand from the room's voice across the day, and tips at end-of-day.
**Success signal:** Kim opens the page in the morning, leaves the tab open all day, comes back at 22:00 to a full jar, taps to tip it, and the gesture feels right (Register A closure).
**Scope IN:** single-file static HTML + Web Audio API analyser; CSS-rendered hourglass; ambient-only detection (no log, no input, no history across days).
**Scope OUT:** auth, server, multi-day history, scoring, gamification, notifications.
**Constraints:** 8GB Mac, static-only, $0.00 cap, no LLM calls.

## Why now

Live mood anchor: AFR 2026-10-05 "Banks jack up fixed rates faster than RBA as funding pain deepens — almost double the 25bp cash-rate hike" + 40-year housing downturn story together create a felt pull toward slower desk-time this week. The sand atom lands on that mood without forcing it.

## Stack

- **Single-file static HTML** — `index.html` only, all CSS + JS inline
- **Web Audio API** — `AudioContext.AnalyserNode.getByteTimeDomainData()` for ambient RMS
- **CSS-only hourglass** — warm-cream canvas, two chambers, gold-accent for flip moment
- **No frameworks** — vanilla JS, vanilla CSS, no build step
- **Hosting** — GitHub Pages, $0

## Register & ambient-only compliance (pitfall #41)

- **Register A** (ceremonial, solo, slow, withholding/delay) — assigned BEFORE mechanic was generated
- **Ambient-only** (2026-09-11 hard rule) — the jar reacts to room volume + time, two things already happening. No log moment. The tip-now gesture is a *closure* moment, not a log moment.

## Build plan

1. Write `index.html` with: hourglass CSS, AudioContext init on first user gesture, 15-min tick (compress via `?tick=15` URL param), threshold-based grain drop, end-of-day flip at 22:00 AEST, "tip now" button.
2. Add `?calibrate=1` URL param that samples room for 30s and sets threshold to 1.5× baseline.
3. Smoke-test (per `~/.hermes/skills/sumi/taro/references/whimsy-poc-smoke-test.md`): load, audio analyser initialises, ticks fire, threshold-above adds grain, threshold-below adds nothing, 22:00 flip animates cascade, tip-now button forces early flip, gold-accent class only during flip, no console errors.
4. Deploy to GitHub Pages via `agentSumi/desk-sand` repo.
5. Verify MD5 byte-identical between local source and served content.

## Evals on success criteria (negative_check discipline)

| # | Criterion | Pass condition | Threshold | How verified |
|---|---|---|---|---|
| 1 | Page loads | HTTP 200 from `/` | 100% | `curl -sI` |
| 2 | Hourglass renders | `#hourglass` element exists with 2 chamber children | 100% | DOM query |
| 3 | AudioContext initialises on first user gesture | `audioCtx.state === 'running'` after click | 100% | browser_exec |
| 4 | Tick fires at interval | 2+ ticks in 30s with `?tick=15` | 100% | browser_exec timer |
| 5 | Above-threshold tick adds 1 grain | grain count increments by exactly 1 | 100% | browser_exec with synthetic input |
| 6 | Below-threshold tick adds 0 grains | grain count unchanged | 100% | browser_exec with synthetic silence |
| 7 | 22:00 flip animates cascade | gold-accent class applies + transform animates | 100% | browser_exec |
| 8 | Tip-now button forces early flip | same cascade animation triggers | 100% | browser_exec |
| 9 | No console errors | `window.onerror` count === 0 | 100% | browser_exec |
| 10 | No LLM calls | no `openai`, `anthropic`, `completion`, `claude` strings in source | 100% | grep |
| 11 | MD5 byte-identical | served MD5 == local MD5 after deploy | 100% | curl + md5sum |
| 12 | No input/log mechanism | no `<input>`, no `localStorage.setItem`, no `fetch` to user data | 100% | grep + DOM audit |

**SHIP verdict requires:** All 12 negative_checks pass.
**ITERATE:** re-derive the threshold calibration affordance, re-run evals.
**SCRAP:** any criterion fails after 2 ITERATE loops.

## Demand-gate (pitfall #43)

"Kim (creative operator under housing-stress mood, wants slow ambient desk rituals this week) wants a tactile day-marker that asks her to do nothing — the sand-trail atom is un-spun in the appeal-library, this is the first attempt."

Named user ✓, named why-now ✓, no vacancy-claim-in-disguise ✓.

## Build type

`personal / whimsy-ambient / Register A` — single-file static HTML.
