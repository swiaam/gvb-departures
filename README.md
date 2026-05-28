# My GVB

A personal departure board for GVB ferry lines in Amsterdam, optimised for mobile use.

Live at: **https://swiaam.github.io/gvb-departures/gvb-departures.html**

## Features

- Real-time departures fetched from the OVapi public transport API
- Countdown timers with colour-coded urgency (green → amber → red)
- Stops grouped by area (West, Centrum, Oost)
- Within each group, stops are automatically sorted by soonest departure
- Auto-refreshes every 30 seconds
- Falls back to demo data when the API is unreachable

## Covered ferry lines

| Line | Route |
|------|-------|
| F2 | IJplein ↔ Centraal |
| F3 | Buiksloterweg (A'dam Tower) ↔ Centraal |
| F4 | NDSM ↔ Centraal |
| F6 | Distelweg ↔ Pontsteiger (West) |
| F7 | NDSM ↔ Pontsteiger (West) |
| F1 | Azartplein (KNSM-eiland) ↔ Zamenhofstraat (Noord) |

## Customising stops

All configuration lives at the top of [`docs/gvb-departures.html`](docs/gvb-departures.html) in two constants:

```js
const STOPS = [
  { id:"f3", label:"Veer Buiksloterweg", lineKey:"GVB_901_1", fromStop:"Buiksloterweg", line:"F3", hint:"A'dam Tower to Centraal" },
  ...
];

const GROUPS = [
  { id:"centrum", label:"Centrum", stopIds:["f2","f3","f4"] },
  ...
];
```

- **lineKey** — OVapi line identifier. The suffix `_1` and `_2` denote opposite directions.
- **fromStop** — filters departures to a specific boarding point (partial match against the API stop name).
- **hint** — small directional label shown on the card as a human-readable hint.

Line keys can be looked up via the [OVapi documentation](http://v0.ovapi.nl).

## Tech stack

- [Preact](https://preactjs.com) + [htm](https://github.com/developit/htm) — no build step required
- Data proxied through [CodeTabs](https://codetabs.com/cors-proxy/cors-proxy.html) to work around CORS restrictions on the OVapi endpoint
- Hosted via GitHub Pages from the `/docs` folder
