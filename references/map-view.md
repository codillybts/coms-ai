# Map View

Map View shows every lead on a map of the territory the business serves. People can click a state to zoom into it, click a county to zoom further, and click any lead to see who it is. It should feel like the best screen in the app: smooth, precise, and good-looking.

Read this when building the CRM (Phase 5), when the user changes their territory, or when they ask about the map.

## What to ask in setup

Round 2 asks where prospects are (R2.3). Then the map questions (R2.3a to R2.3c in `intake-interview.md`) pin down the territory:

1. **Country.** US, another country, or several countries.
2. **US area.** The whole US, a list of states, or a metro or set of counties.
3. **Counties.** Only if they named 1 to 3 states, or a metro: which counties? Claude can suggest the counties for a metro name ("Denver metro" means Denver, Arapahoe, Adams, Jefferson, Boulder, Douglas and Broomfield counties) and the user confirms.

Save to `territory` in the profile:

```json
"territory": {
  "label": "Denver metro",
  "country": "US",
  "level": "counties",
  "states": ["08"],
  "counties": ["08031", "08005", "08001", "08059", "08013", "08035", "08014"],
  "countries": []
}
```

- `states` are 2-digit US state FIPS codes. `counties` are 5-digit county FIPS codes.
- `level` is `us`, `states`, or `counties` for US territories, or `countries` for anything outside the US.
- `label` is what the map shows as the territory name.

## How the map behaves

**Starting view**
- 4 or more states: opens on the US map, zoomed to the territory.
- 1 to 3 states: opens on the state map (the first state), with counties drawn.
- Counties territory: opens on the state map, with the territory counties outlined.
- Outside the US: opens on a world map zoomed to the chosen countries (see "Outside the US").

**Levels**
1. **US view.** All states are drawn. Territory states are raised with a soft shadow and shaded by how many leads they hold. Other states are pale gray. Each territory state shows its two-letter code.
2. **State view.** Click a state. The map flies to it and draws its counties, shaded by lead count. Other states fade. Clicking a state outside the territory still works and says "Outside your territory".
3. **County view.** Click a county. The map flies to it. Other counties fade.

A breadcrumb at the top (United States > Colorado > Weld County) goes back up. Esc also goes back up one level. Scroll or pinch zooms freely, and the + / − and "Whole territory" buttons sit in the corner.

**Leads**
- Each lead is a dot colored by status: New (brand color), Contacted (gray), Replied (amber), Interested (violet), Won (green). Interested and Won dots pulse softly.
- Dots keep the same size on screen at every zoom level.
- Clicks pick the nearest lead within about 11 screen pixels (use a Delaunay index, not the dot's own hit area), so small dots are easy to hit even in dense clusters. A click that is not near a lead selects the state or county under it.
- Hovering shows a small tooltip: lead name, company and status, or the area name and lead count.
- Clicking a lead marks it with a ring and shows its card at the top of the side panel: name, title, company, city and county, status, published email and where it was found, fit, deal value, and buttons (Write first email, Open conversation, Zoom here). The card never covers the map.

**Side panel**
- Area name and level (Your territory, State view, County view).
- Four numbers for the current area: leads, share with a published email, leads in conversation, interested pipeline value.
- Show: Leads (dots) or Density (shading only).
- Status legend that doubles as a filter, with counts.
- "Find leads here" opens Find leads pre-filled with this area.
- "Write first emails (N)" for New leads with a published email in this area.
- "Best fits here": the top leads by fit score. Clicking one flies to it.
- While a lead search is running, new leads drop onto the map with a ripple and appear in a short feed.

**Never**
- Never plot a lead at an address that was guessed. See "Where a lead's dot goes" below.
- Never show Valid, Risky or Invalid on the map unless a connected email checker tested that email.

## Where a lead's dot goes

Agents record a location for every lead, using the same no-guessing rule as emails:

1. The business's published street address (website footer, contact page, a public registry or directory listing). Convert it to latitude and longitude with any geocoder the user has connected, or a free public one that allows this use. Save `location.precision: "address"`.
2. If only the city is published, use the city's center. Save `location.precision: "city"` and draw that dot slightly faded.
3. If nothing is published, leave the location empty. The lead appears in lists but not on the map, and the side panel says how many are unplaced.

Store `location.lat`, `location.lon`, `location.state_fips`, `location.county_fips`, `location.city`, and `location.precision` on each lead. The page projects latitude and longitude itself, so the stored data stays plain.

## How to build it

- Libraries: d3 7.9.0 and topojson 3.0.2 from cdnjs (pinned versions).
- Map data ships with the skill in `assets/maps/` and is published next to the page:
  - `us-counties-albers-10m.json` (US Atlas: states, counties, nation; already projected, 975 x 610).
  - `world-countries-110m.json` (World Atlas, for territories outside the US).
- US projection for lead dots: `d3.geoAlbersUsa().scale(1300).translate([487.5, 305])`. This matches the projected US Atlas file.
- Size the SVG's viewBox to the map area's shape so the map fills it, and refit on window resize.
- Read colors from the theme tokens and the brand color, so the map matches the rest of the CRM.
- Map View is already built into `assets/app.html`. Only change it if the user asks.

## Outside the US

For other countries, use `world-countries-110m.json` with `d3.geoNaturalEarth1()` (or `geoMercator` for a single small country), fitted to the territory countries. Levels are World > Country. Regions inside other countries need a boundary file for that country; ask the user before adding one, and tell them where it comes from.

## Changing the territory later

The user says "add Texas to my territory" or "change my territory to the Denver metro". Update `territory` in the profile and in the CRM's settings, then reload the page. Leads outside the new territory stay in the CRM. They are only shown faded on the map.
