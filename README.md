# Weird Science – Band Availability

A simple, static availability dashboard for the band **Weird Science**, showing who is available on which dates at a glance.

The entire system is deliberately minimal:
- no backend
- no database
- no authentication
- no build step

Live site:  
👉 https://ws.echo3.co

---

## Architecture (intentionally simple)

Power Automate
↓
Google Sheet
↓ (Published as CSV)
Static HTML (GitHub Pages)


That’s it.

- **Power Automate** writes availability into a Google Sheet
- The sheet is **published to the web as a CSV**
- A single **static HTML file** fetches and renders the data

No servers, no secrets, no fragile integrations.

---

## How it works

- `index.html` fetches a public CSV published from Google Sheets
- Only three columns are used:
  - `member`
  - `startdate`
  - `enddate`
- Date ranges are expanded client-side into individual unavailable days
- The UI renders:
  - 12 monthly calendars
  - a filter (whole band or per member)
  - a raw unavailability list for audit/reference

Everything runs in the browser.

---

## Availability rules

### Whole band mode (default)

- ✔ Available: **nobody** is unavailable
- ✖ Unavailable: **one or more** members are unavailable

Hovering an unavailable date shows a list of unavailable members.

### Per member mode

- ✔ / ✖ reflects **that member only**
- Tooltips still show the full list of unavailable members for that date

---

## Accessibility

Designed to work well for red/green colour blindness:

- Blue = available
- Amber = unavailable
- ✔ / ✖ icons used as non-colour cues
- Unavailable dates use a striped texture
- Tooltips only appear on unavailable dates

---

## Google Sheet requirements

The published sheet must include a header row.

Only these columns are used:

| Column      | Required | Notes |
|------------|----------|-------|
| `member`   | Yes      | Band member name |
| `startdate`| Yes      | Start of unavailability |
| `enddate`  | Yes      | End of unavailability (same as startdate for single-day absences) |

### Date formats supported

- `YYYY-MM-DD` (recommended)
- `DD/MM/YYYY`

---

## Repo structure

/
├── index.html
├── README.md
└── CNAME


There is **no data stored in the repo**.

---

## Updating availability

1. Power Automate updates the Google Sheet
2. The published CSV updates automatically
3. The site reflects changes immediately on refresh

No redeploy, no commits, no workflow runs required.

---

## Why this approach?

This design intentionally avoids:
- APIs with auth
- server-side code
- client-side secrets
- complex tooling

The result is:
- extremely reliable
- very easy to reason about
- free to host
- future-proof

If Google Sheets can serve a CSV, this site will keep working.

---

## Licence

Private project for band use.  
All rights reserved.
