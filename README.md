# Kovai Connect

A local, community-first bus guide for Coimbatore. It includes a searchable route directory, route-endpoint suggestions where the published endpoints line up, address-level transit handoff to Google Maps, and a small data desk for maintaining route records.

## Run the demo

Requires Node.js 18 or newer. No package installation is needed.

```sh
npm start
```

Open <http://localhost:4173> in a browser. The route database is stored in `db/routes.json`; changes made in **Data desk** are written to that file. `db/schema.sql` describes a normalized SQLite schema for a future hosted version.

The planner also works when `public/` is served as a static site or opened as a local preview: it loads the bundled route data and saves route-desk edits in that browser. The snapshots are refreshed whenever the Node server writes the main database.

## Embed Google Maps

The results page displays an interactive Google Maps transit directions map when a Maps Embed API key is configured. Google requires an API key and a billing account; Maps Embed API usage is currently listed as no-charge. Restrict the key to **Maps Embed API** and the website referrers you use (for local development, include `http://localhost:4173/*`).

For the Node app in PowerShell, set the key before starting the server:

```powershell
$env:GOOGLE_MAPS_API_KEY = "YOUR_RESTRICTED_KEY"
npm start
```

For static hosting, put the key in `public/maps-config.js` as the value of `window.KOVAI_MAPS_API_KEY`. This is a browser-visible key, so keep the API and website restrictions enabled. Without a key, the results still offer the Google Maps transit link in a new tab.

## Demo data

The route directory expands the 257 grouped entries in the user-provided `Coimbatore_Bus_Routes_and_Timings.pdf` into 273 individually searchable bus-code entries. Route endpoints are cross-checked against the [CCMC Bus Services directory](https://ccmc.gov.in/index.php/createpage/bus-services) and number variants and unclear spellings against the [Coimbatore Junction 2026 directory](https://coimbatorejunction.in/coimbatore-bus-routes/). These published lists mostly provide endpoints, not a comprehensive stop sequence or reliable town-bus timetable. Timings and journey durations are left blank rather than guessed. Route 111 includes separately sourced loop landmarks, with direction still requiring confirmation.

## Included features

- Direct and up to two-transfer route search using listed stops/endpoints, including inferred return directions. Route 111 includes separately sourced landmark stops.
- Boarding and destination suggestions filter each other so listed choices only show pairs that have a route in the current data (up to three buses).
- Typed addresses, landmarks, or listed stop suggestions as journey points; an optional geolocation starting point.
- An embedded Google Maps transit directions map when configured, with an external link fallback. Transit coverage depends on the journey.
- Route directory search by route number or origin/destination.
- Mobile responsive layout and precise location-assisted starting point.
- Local route data desk to add and remove route records.
- JSON API at `/api/routes` and `/api/plan`.
- Summit-ready explanation of the problem, public benefit, and future scope.

Before deploying beyond a local demo, connect verified operator data, add intermediate stops and timetable source timestamps, and secure route editing.
