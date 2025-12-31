# Weird Science – Band Availability

A simple, static, zero-backend availability calendar for the band **Weird Science**, hosted on GitHub Pages.

This site displays band availability at a glance using monthly calendars, with the underlying unavailability data stored as a single JSON file in the repo.

Designed to be:
- fast
- free to host
- easy to maintain
- accessible (colour-blind safe)

Live site:  
👉 https://ws.echo3.co

---

## How it works

- The site is a single static `index.html`
- Availability data lives in `/data/availability.json`
- JavaScript fetches the JSON and renders:
  - 12 monthly calendars
  - a filter (whole band or per member)
  - a raw unavailability list for audit / reference

There is **no backend**, **no database**, and **no build step**.

GitHub Pages serves everything directly.

---

## Availability rules

### Whole band mode (default)

- ✔ Available: **nobody** is unavailable
- ✖ Unavailable: **one or more** members are unavailable

Hovering an unavailable date shows **only the names of unavailable members**.

### Per member mode

- ✔ / ✖ reflects **that member only**
- Tooltips still show who is unavailable on that date

---

## Accessibility

This project is designed to work well for red/green colour blindness:

- Uses **blue (available)** and **amber (unavailable)**
- Uses **icons (✔ / ✖)** as non-colour cues
- Uses **striped texture** on unavailable dates
- Available dates have **no tooltip**, reducing visual noise

---

## Repo structure

