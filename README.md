# Weird Science – Band Availability

A simple, static availability dashboard for the band **Weird Science**, showing who is available on which dates at a glance.

What started as a pure calendar has evolved into a **lightweight, arcade-style interaction layer** that makes checking availability oddly satisfying, without compromising simplicity or reliability.

Live site:
👉 [https://ws.echo3.co](https://ws.echo3.co)

---

## Core principles

The entire system is deliberately minimal:

* no backend
* no database
* no authentication
* no build step
* no frameworks

Everything runs client-side in a single static HTML file.

---

## Architecture (intentionally simple)

Power Automate
↓
Google Sheet
↓ (Published as CSV)
Static HTML (GitHub Pages)

That’s it.

* **Power Automate** writes availability into a Google Sheet
* The sheet is **published to the web as a CSV**
* A single **static HTML file** fetches, parses, and renders the data

No servers, no secrets, no fragile integrations.

---

## How it works

* `index.html` fetches a public CSV published from Google Sheets
* Only three columns are used:

  * `member`
  * `startdate`
  * `enddate`
* Date ranges are expanded client-side into individual unavailable days
* The UI renders:

  * 12 monthly calendars
  * a filter (whole band or per member)
  * a raw unavailability list for audit/reference

Everything runs entirely in the browser.

---

## Availability rules

### Whole band mode (default)

* ✔ Available: **nobody** is unavailable
* ✖ Unavailable: **one or more** members are unavailable

Hovering an unavailable date shows a list of unavailable members.

### Per member mode

* ✔ / ✖ reflects **that member only**
* Tooltips still show the full list of unavailable members for that date

---

## Game layer (optional, client-side only)

The calendar doubles as a **light arcade game** inspired by classic fruit-machine mechanics.
This layer is entirely cosmetic and does not affect availability logic.

### Game concepts

* Each month is treated as a “board”
* Each day tile can be:

  * **Cleared** (available)
  * **Blocked** (unavailable)
* Clearing all valid days in a month completes that month
* Difficulty increases as the year progresses

### Difficulty scaling

* Later months allow **less total time**
* Bomb / penalty frequency increases
* Player precision matters more in later months

Game progression is deterministic and stateless; nothing is stored remotely.

---

## Sound design

All audio is generated **in real time** using the Web Audio API.

No audio files are used.

### Sound system characteristics

* Synth-generated tones only
* Chip-style envelopes inspired by 8-bit and early arcade hardware
* Zero external assets
* Minimal CPU footprint

### Sound events include

* Coin insert / game start
* Tile interaction
* Successful clears
* Penalty / bomb triggers
* Month completion
* Subtle ambient music layers

Audio is muted by default until the user interacts, in line with browser autoplay rules.

---

## Accessibility

Designed to work well for red/green colour blindness:

* Blue = available
* Amber = unavailable
* ✔ / ✖ icons used as non-colour cues
* Unavailable dates use a striped texture
* Tooltips only appear on unavailable dates

Game elements do not override accessibility cues.

---

## Google Sheet requirements

The published sheet must include a header row.

Only these columns are used:

| Column      | Required | Notes                                                             |
| ----------- | -------- | ----------------------------------------------------------------- |
| `member`    | Yes      | Band member name                                                  |
| `startdate` | Yes      | Start of unavailability                                           |
| `enddate`   | Yes      | End of unavailability (same as startdate for single-day absences) |

### Date formats supported

* `YYYY-MM-DD` (recommended)
* `DD/MM/YYYY`

---

## Repo structure

```
/
├── index.html
├── README.md
└── CNAME
```

There is **no data stored in the repo**.

---

## Updating availability

1. Power Automate updates the Google Sheet
2. The published CSV updates automatically
3. The site reflects changes immediately on refresh

No redeploys, no commits, no workflows required.

---

## Why this approach?

This design intentionally avoids:

* APIs with authentication
* server-side code
* client-side secrets
* complex tooling
* fragile dependencies

The result is:

* extremely reliable
* trivial to debug
* free to host
* resilient to partial failures
* future-proof

If Google Sheets can serve a CSV, this site will keep working.

---

## Licence

Private project for band use.
All rights reserved.
