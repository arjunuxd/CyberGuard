# 🛡️ CyberGuard — Cybersecurity Awareness Mission

An interactive, single-file HTML experience that teaches cybersecurity awareness through a gamified "mission control" interface — complete with a mission map, badges, quizzes, and two fully simulated hacking labs.

No build tools, no dependencies, no backend. Open the file in a browser and it runs.

---

## What it is

CyberGuard turns cybersecurity training into a short, self-contained game. The learner lands on a simulated "suspicious login" alert, accepts the mission, and works through a radial map of 12 nodes covering threats, warning signs, incident response, and two **hands-on simulation labs** — one where they watch a weak password get cracked in a live terminal, and one where a fake "cracked game" download hijacks the page with a scripted ransomware sequence.

Everything is self-paced, mouse- and keyboard-navigable, and built to *feel* like a hacking interface (terminal windows, scanlines, glitch effects, click ripples) without ever touching a real network or real data.

---

## Features

- **Mission map** — 12 radial nodes around a central hub, with animated connecting lines that light up as you complete each one. Falls back to a vertical timeline on mobile.
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
- **Progress tracking** — sidebar tracker, badge collection, and a top progress bar, all driven by which nodes you've visited.
- **Sound effects** — synthesized in-browser via the Web Audio API (clicks, key taps, success/fail tones, alarms) — no audio files required.
- **Hack-vibe click feedback** — every click anywhere on the page fires a cyan ripple and a floating glyph (`0x4F2A`, `SYN`, `ssh://`, etc.).
- **Accessible** — keyboard navigation (arrow keys + Enter on the map, Esc to close panels), `prefers-reduced-motion` support, focus outlines, ARIA roles on the dialog panel.
- **Responsive** — radial map + sidebar on desktop, a vertical scrolling timeline on mobile/narrow viewports.

### Simulation Lab 1 — Password Cracking

1. Create a practice username + password (a live strength meter scores it as you type).
2. Click **Hack It**.
3. A terminal window pops up and asks for the target username — type it and press Enter.
4. The terminal runs a simulated dictionary attack, then escalates to brute-force if needed, printing each attempt live.
5. **Weak passwords** (e.g. `arjun123`, anything from a common-password list, or anything short/predictable) get cracked, with an explanation of why.
6. **Strong passwords** survive, with an explanation of why length + randomness wins.

Everything here is computed client-side with a small built-in word list — no real cracking, no real accounts.

### Simulation Lab 2 — The Cracked Game (Ransomware)

1. A fake "cracked game" download card is presented, clearly labeled as part of the simulation.
2. Clicking **Download** runs a fake progress bar.
3. Clicking **Install** triggers a full-screen takeover: a flash, a "SYSTEM COMPROMISED" popup, then a glitching screen with a jittering fake cursor and a terminal that "executes" a scripted ransomware chain (disabling defenses, "encrypting" files, dropping a ransom note).
4. Press **Esc** or **Enter** at any point to stop it.
5. A debrief screen explains exactly which real-world warning signs were skipped and what actually prevents this (offline backups, real-time protection, official sources only).

Nothing is downloaded, executed, or sent anywhere — it's DOM/CSS/JS animation only.

---

## Getting started

No installation needed.

1. Download `cyberguard.html`.
2. Open it directly in any modern browser (double-click, or drag into a browser window).
3. Click **Start Mission** and explore the map.

To host it (e.g. for a classroom or team training session), just upload the single HTML file to any static web host — GitHub Pages, Netlify, an internal file share, etc. There is no server-side component.

---

## Project structure

This is intentionally a **single self-contained HTML file** — all CSS and JavaScript are inline, with no external dependencies except two Google Fonts loaded via `<link>` (Rajdhani + IBM Plex Mono + Inter).

```
cyberguard.html   ← everything: markup, styles, and logic
```

If you want to split it up for easier editing, the natural seams are:

| Section | What's there |
|---|---|
| `<style>` block | Design tokens (`:root` CSS variables), layout, components, the two simulation-lab UI kits, and responsive rules |
| Boot / Alert / App markup | The loading screen, mission-briefing screen, and the main dashboard shell |
| `NODES` array | The 12 mission nodes — id, icon, label, badge info |
| `builders` object | One function per node id, returning the HTML injected into the detail panel |
| `wireInteractive(id)` | Attaches click handlers for whichever node's content was just rendered |
| Sim Lab 1 functions | `simCred`, `pwStrengthScore`, `launchCredTerminal`, `runCrackSequence`, `termPrintLines` |
| Sim Lab 2 functions | `simRansom`, `runFakeDownload`, `runFakeInstall`, `startHackTerminalSequence`, `endHackSequence` |

---

## Browser support

Built with vanilla JS (ES5/ES6 mix, no transpilation needed) and standard CSS (Grid, custom properties, `backdrop-filter`). Works in current versions of Chrome, Edge, Firefox, and Safari. `backdrop-filter` blur effects degrade gracefully (panels just lose the frosted-glass look) in older browsers that don't support it.

---

## Customization

- **Add a node:** add an entry to the `NODES` array, write a matching `builders.yourId = function(){ ... }`, and (if it needs interactivity) add a case to `wireInteractive(id)`.
- **Change the quiz:** edit the `QUIZ` array — each entry is `{ q, opts, correct }`.
- **Adjust password rules for the cracking lab:** edit `COMMON_PASSWORDS` and `pwStrengthScore()`.
- **Retheme:** everything visual is driven by the CSS custom properties at the top of the `<style>` block (`--cyan`, `--danger`, `--success`, fonts, radii, etc.) — change those and the whole UI updates.
- **Disable sound:** the speaker icon in the top bar toggles all SFX at runtime; there's nothing to configure ahead of time.

---

## Notes for facilitators

- Both simulation labs are explicitly labeled as simulations in their own UI copy, and the ransomware lab's debrief screen recaps the real warning signs that were present.
- No real network requests, downloads, or system changes occur anywhere in either lab.
- The ransomware lab includes screen-shake and a flashing red overlay — worth a heads-up if running this with anyone sensitive to that kind of visual effect (it respects `prefers-reduced-motion`, but only for users who have that OS-level setting enabled).

---

## License

Use, adapt, and redistribute freely for educational and training purposes.