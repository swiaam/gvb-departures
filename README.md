# My GVB

A personal departure board for GVB ferry lines in Amsterdam, optimised for mobile use as a PWA.

Live at: **https://swiaam.github.io/gvb-departures/gvb-departures.html**

## Features

- Real-time departures fetched from the OVapi public transport API
- Countdown timers with colour-coded urgency (green → amber → red)
- Stops grouped by direction (Noord → West, West → Noord, Centrum, Oost)
- Within each group, stops are automatically sorted by soonest departure
- Shows N/A with error reason if the API is unreachable
- Auto-refreshes every 30 seconds

## Covered ferry lines

| Line | Route |
|------|-------|
| F2 | IJplein ↔ Centraal |
| F3 | Buiksloterweg (A'dam Tower) ↔ Centraal |
| F4 | NDSM ↔ Centraal |
| F6 | Distelweg ↔ Pontsteiger |
| F7 | NDSM ↔ Pontsteiger |
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
- **hint** — short directional label shown on the card next to the stop name.

Line keys can be looked up via the [OVapi documentation](http://v0.ovapi.nl).

## Architecture

| Concern | Solution |
|---------|----------|
| UI | Single HTML file, no build step |
| Frontend framework | [Preact](https://preactjs.com) + [htm](https://github.com/developit/htm), vendored locally in `docs/vendor/` |
| Static hosting | GitHub Pages from `/docs` |
| API proxy | Google Cloud Function (`europe-west4`) — works around CORS on OVapi, restricted to requests from this domain |
