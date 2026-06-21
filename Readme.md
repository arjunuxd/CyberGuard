# 🛡️ CyberGuard — Cybersecurity Awareness Mission

An interactive, single-file HTML experience that teaches cybersecurity awareness through a gamified "mission control" interface — complete with a radial mission map, badges, a quiz, and two fully simulated hacking labs that react to whatever the learner actually types.

No build tools, no dependencies, no backend. Open the file in a browser and it runs.

---

## What it is

CyberGuard turns cybersecurity training into a short, self-paced game. The learner lands on a simulated "suspicious login" alert, accepts the mission, and works through a radial map of 12 nodes covering threats, warning signs, incident response, and two **hands-on simulation labs**:

- A **password-cracking lab** where the learner creates their own test account and watches a terminal genuinely attempt to crack whatever password they typed — not a scripted example.
- A **ransomware lab** where a fake "cracked game" download hijacks the page with an intense, fully simulated intrusion sequence.

Everything is self-paced, mouse- and keyboard-navigable, and built to *feel* like a real hacking interface (terminal windows, scanlines, glitch effects, click ripples) without ever touching a real network, real files, or real data.

---

## Features

- **Radial mission map** — 12 nodes around a central hub, with animated connecting lines that light up as each one is completed. Falls back to a scrollable vertical timeline on mobile.
- **12 content nodes:**
  1. Cybersecurity Fundamentals
  2. Common Cyber Threats (expandable cards)
  3. Warning Signs (2 interactive challenges)
  4. Incident Response (checkable step flow)
  5. Financial Fraud Response
  6. Reporting Cybercrime
  7. Prevention Strategies (live password-strength tester)
  8. Real-Life Case Study (scenario + challenge question)
  9. **Simulation Lab — Password Cracking**
  10. **Simulation Lab — The Cracked Game (ransomware)**
  11. Cybersecurity Quiz (5 questions, scored)
  12. Mission Complete (summary + key takeaways)
- **Progress tracking** — sidebar tracker, badge collection, and a top progress bar, all driven by which nodes have been visited.
- **Synthesized sound effects** — generated live via the Web Audio API (clicks, key taps, success/fail tones, alarms, a dissonant drone, a heartbeat thud) — no audio files required.
- **Hack-vibe click feedback** — every click anywhere on the page fires a cyan ripple and a floating glyph (`0x4F2A`, `SYN`, `ssh://`, etc.).
- **Accessible** — keyboard navigation (arrow keys + Enter on the map, Esc to close panels), `prefers-reduced-motion` support throughout (including the ransomware lab's glitch effects), focus outlines, ARIA roles on the dialog panel.
- **Responsive** — radial map + sidebar on desktop, a full-height scrollable timeline on mobile, with touch-scroll hardening (`-webkit-overflow-scrolling`, `overscroll-behavior`) so long content reliably scrolls on phones.

---

### Simulation Lab 1 — Password Cracking

This lab evaluates **whatever password the learner actually types** — it is not limited to a fixed example.

1. Create a practice username + password. A live strength meter scores it as you type.
2. Click **Hack It**.
3. A terminal pops up and asks for the target username — type it and press Enter.
4. The terminal runs a simulated dictionary + pattern attack, escalating to a brute-force timing estimate if nothing obvious matches, printing each attempt live.
5. The crack/no-crack verdict always matches what the strength meter already showed:

   | Strength label | Score range | Outcome in the lab |
   |---|---|---|
   | Very weak | 0–24 | Cracked — usually an exact dictionary hit |
   | Weak | 25–49 | Cracked — usually a recognizable pattern |
   | Moderate | 50–74 | **Cracked** — no obvious pattern, but the keyspace is small enough to fall to a sustained attack |
   | Strong | 75–100 | **Holds** — brute-force times out |

   Only genuinely **Strong** passwords survive. Anything Weak or Moderate gets cracked, with the terminal explaining *why* (e.g. "a common first name with digits tacked on the end," "a keyboard-walk pattern," or — for moderate passwords with no obvious pattern — a keyspace-exhaustion explanation).

The detection engine catches more than literal string matches:
- A large list of breached/common passwords and common first names
- **Leetspeak normalization** (`@`→a, `0`→o, `3`→e, `5`→s, `1`→i, `7`→t, `8`→b) so `P@ssw0rd` is recognized as `password`
- **Word/name + digits** patterns (`Arjun2024`, `rahul99`, `Summer2024!`)
- **Keyboard walks** (`qwerty`, `qazwsx`, `asdfgh`, etc.)
- **Repeated characters and simple numeric sequences** (`aaaaaaaa`, `12345678`)

This means a teacher can call any student up, have them invent any password they like, and the lab will give a consistent, honest, explainable verdict — including for passwords no one has ever typed before.

Everything here runs client-side with built-in word lists. No real cracking tool is used, no real accounts exist, and nothing leaves the browser.

---

### Simulation Lab 2 — The Cracked Game (Ransomware)

This lab simulates a trojanized "cracked game" download and its aftermath, designed to be genuinely tense without being gratuitous (no gore, no real jump-scare audio blast, no actual webcam/microphone access of any kind — that line in the terminal is explicitly logged as denied).

1. A "cracked game" download card is presented, clearly part of the simulation.
2. Clicking **Download** runs a fake progress bar.
3. Clicking **Install** triggers a short build-up of screen-static flicker bursts, then a full-screen "SYSTEM COMPROMISED" popup with a pulsing "INTRUSION DETECTED" badge.
4. Continuing opens a glitching, full-screen terminal:
   - A persistent "SIMULATION ACTIVE" recording-style indicator stays in the corner for the duration.
   - The terminal "scans" for files, disables fake defenses, and begins an encryption pass with a live encrypted-file counter climbing alongside a progress bar.
   - The screen shake, color distortion, and audio (a low dissonant drone, a heartbeat thud, harsher glitch tones) all intensify together as the encryption percentage climbs, landing on a dissonant audio "sting" and a hard red flash at 100%.
   - A ransom note is displayed.
5. **Esc** or **Enter** stops the sequence at any point.
6. A debrief screen explains exactly which real-world warning signs were present (pirated software from an unverified host, an installer that wants to disable antivirus, etc.) and what actually prevents this (offline backups, real-time protection, official sources only).

Nothing is downloaded, executed, or sent anywhere — it's DOM/CSS/JS animation and synthesized audio only. The whole sequence respects `prefers-reduced-motion`, dropping the screen-shake and filter effects for users with that OS-level setting enabled.

---

## Getting started

No installation needed.

1. Download `cyberguard.html`.
2. Open it directly in any modern browser (double-click, or drag into a browser window).
3. Click **Start Mission** and explore the map.

To host it (e.g. for a classroom or team training session), upload the single HTML file to any static web host — GitHub Pages, Netlify, an internal file share, etc. There is no server-side component.

---

## Project structure

This is intentionally a **single self-contained HTML file** — all CSS and JavaScript are inline, with no external dependencies except three Google Fonts loaded via `<link>` (Rajdhani, IBM Plex Mono, Inter).

```
cyberguard.html   ← everything: markup, styles, and logic
```

If you want to split it up for easier editing, the natural seams are:

| Section | What's there |
|---|---|
| `<style>` block | Design tokens (`:root` CSS variables), layout, components, the two simulation-lab UI kits, and responsive/touch-scroll rules |
| Boot / Alert / App markup | The loading screen, mission-briefing screen, and the main dashboard shell |
| `NODES` array | The 12 mission nodes — id, icon, label, badge info |
| `builders` object | One function per node id, returning the HTML injected into the detail panel |
| `wireInteractive(id)` | Attaches click handlers for whichever node's content was just rendered |
| **Password lab** | `COMMON_PASSWORDS`, `COMMON_NAMES`, `KEYBOARD_WALKS`, `leetNormalize()`, `stripTrailingNoise()`, `classifyWeakness()`, `pwStrengthScore()`, `pwStrengthLabel()`, `launchCredTerminal()`, `runCrackSequence()`, `showCredResult()` |
| **Ransomware lab** | `simRansom`, `runFakeDownload()`, `runFakeInstall()`, `triggerHackTakeover()`, `startHackTerminalSequence()`, `hackTermPrint()`, `animateFakeCursor()`, `endHackSequence()`, plus the dread-focused SFX: `sfxDrone()`, `sfxThud()`, `sfxGlitchHard()`, `sfxSting()` |

---

## Browser support

Built with vanilla JS (ES5/ES6 mix, no transpilation needed) and standard CSS (Grid, custom properties, `backdrop-filter`). Works in current versions of Chrome, Edge, Firefox, and Safari, including mobile. `backdrop-filter` blur effects degrade gracefully in older browsers that don't support it.

---

## Customization

- **Add a node:** add an entry to the `NODES` array, write a matching `builders.yourId = function(){ ... }`, and (if it needs interactivity) add a case to `wireInteractive(id)`.
- **Change the quiz:** edit the `QUIZ` array — each entry is `{ q, opts, correct }`.
- **Adjust the password-cracking thresholds:** edit `pwStrengthScore()`/`pwStrengthLabel()` to change scoring, or the `willCrack = score < 75` line in `runCrackSequence()` to change which strength tiers get cracked.
- **Expand the dictionary:** add entries to `COMMON_PASSWORDS`, `COMMON_NAMES`, or `KEYBOARD_WALKS`.
- **Tune the ransomware intensity:** the escalation is driven by the `escalate:true` flags on lines inside `startHackTerminalSequence()`'s `lines` array, and the `.glitching` / `.glitching-intense` CSS classes control the visual intensity tiers.
- **Retheme:** everything visual is driven by the CSS custom properties at the top of the `<style>` block (`--cyan`, `--danger`, `--success`, fonts, radii, etc.) — change those and the whole UI updates.
- **Disable sound:** the speaker icon in the top bar toggles all SFX at runtime; nothing needs to be configured ahead of time.

---

## Notes for facilitators

- Both simulation labs are explicitly labeled as simulations in their own UI copy, and the ransomware lab's debrief screen recaps the real warning signs that were present.
- No real network requests, downloads, webcam/microphone access, or system changes occur anywhere in either lab — the "webcam handle" line in the ransomware terminal is shown being denied by the sandbox.
- The ransomware lab includes screen-shake, color-shift filters, a flashing red overlay, and low-frequency audio (a drone and a heartbeat thud) building to a dissonant sting — worth a heads-up if running this with anyone sensitive to that kind of visual or audio intensity. It respects `prefers-reduced-motion`, but only for users who have that OS-level setting enabled; consider toggling the sound-off button via the speaker icon for audio-sensitive settings, since there's no separate "low intensity" toggle.
- The password lab uses real (but non-identifying) passwords as part of its dictionary — nothing typed by a learner is stored, logged, or sent anywhere; it's scored entirely in the browser and discarded when the panel closes.

---

## License

Use, adapt, and redistribute freely for educational and training purposes.