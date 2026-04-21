# IPG Representative Finder

Interactive map tool for finding elected representatives across Illinois and Chicago. Built for the [Institute for the Public Good](https://publicgoodpolicy.org) and embedded on their Squarespace site via iframe.

**Live URL:** [https://publicgoodpolicy.github.io/ipg-rep-finder/](https://publicgoodpolicy.github.io/ipg-rep-finder/)

---

## Layers

| Tab | Coverage | Roster size |
| :---- | :---- | :---- |
| IL House | All of Illinois | 118 members |
| IL Senate | All of Illinois | 59 members |
| Congressional | All of Illinois | 17 members |
| Chicago Wards | City of Chicago only | 50 alderpersons |
| School Board | City of Chicago only | 20 subdistricts (A/B) |
| Cook County | Cook County | 17 commissioners |
| Police Councils | City of Chicago only | 22 districts, 3 members each |

---

## Files

| File | Description | Source |
| :---- | :---- | :---- |
| `index.html` | Main application — all roster data and JS embedded here | This repo |
| `house.geojson` | IL House district boundaries | Census Bureau TIGER 2024 `tl_2024_17_sldl.zip` — field: `SLDLST` |
| `senate.geojson` | IL Senate district boundaries | IL Senate Redistricting Committee — SB 927 HFA \#2 PA 102-0663 — field: `DISTRICTN` |
| `congress.geojson` | Federal congressional boundaries | Census Bureau TIGER 2024 `tl_2024_17_cd119.zip` — field: `CD119FP` |
| `wards.geojson` | Chicago ward boundaries | City of Chicago Data Portal dataset `p293-wvbd` — field: `WARD_NUM` |
| `schoolboard.geojson` | Chicago school board subdistricts (20 A/B) | IL Senate Redistricting Committee `ERSB_20_Sub_District_Map_FA1_SB_15.zip` — field: `LONGNAME` |
| `cookcounty.geojson` | Cook County commissioner districts | Cook County Open Data Hub — Commissioner District Current — field: `DISTRICT_INT` |
| `police.geojson` | Chicago police district boundaries | City of Chicago Data Portal dataset `fthy-xz3r` — field: `dist_num` |

---

## How to Update Roster Data

All roster data lives inside `index.html` in the `const REPS = { ... }` object near the top of the `<script>` block.

### Updating a single member

Find the district number and update the fields. Example for Ward 35:

35: { name: "Anthony J. Quezada", party: "D", district: "35th Ward", phone: "(773) 887-3772", email: "ward35@cityofchicago.org", website: "" },

### Field reference

**House / Senate / Congress / Wards / Cook County:**

- `name` — Full name  
- `party` — `"D"`, `"R"`, or `""` for nonpartisan  
- `district` — Display label (e.g. `"35th Ward"`)  
- `phone` — Phone number string  
- `email` — Direct email (shows Send Email button if present)  
- `website` — Official website URL (shows Contact via Website button if no email)

**School Board** (keyed by subdistrict string e.g. `'1a'`, `'1b'`):

- `name`, `role` (`"Elected"` or `"Appointed"`), `district`, `phone`, `email`, `website`

**Police District Councils** (keyed by district number):

- `chair`, `nominating`, `community` — member names by role  
- `district` — display label  
- `website` — link to CCPSA district page

### Authoritative sources

| Layer | Source |
| :---- | :---- |
| IL House | [https://www.ilga.gov/house/default.asp](https://www.ilga.gov/house/default.asp) |
| IL Senate | [https://www.ilga.gov/senate/default.asp](https://www.ilga.gov/senate/default.asp) |
| Congressional | [https://www.house.gov/representatives](https://www.house.gov/representatives) |
| Chicago Wards | City of Chicago Data Portal — Ward Offices (export as CSV) |
| School Board | [https://www.cpsboe.org/about/bios](https://www.cpsboe.org/about/bios) |
| Cook County | [https://www.cookcountyil.gov/all-people](https://www.cookcountyil.gov/all-people) |
| Police Councils | [https://ccpsa.chicago.gov/about-the-district-councils/](https://ccpsa.chicago.gov/about-the-district-councils/) |

---

## How to Update Boundary Files

1. Download the new shapefile from the source in the Files table above  
2. Go to **mapshaper.org** and drag the ZIP in  
3. Click Console, type `info`, confirm the district number field name matches what is in `getDistrictNum()` in `index.html`  
4. Export as GeoJSON  
5. Upload to this repo replacing the existing file

If the field name changes, update the corresponding block inside `getDistrictNum()` in `index.html`.

---

## How to Add a New Layer

1. Add a tab button in the `<div class="layer-tabs">` section:  
     
   \<button class="layer-tab" onclick="switchLayer('newlayer',this)"\>Label\</button\>  
     
2. Add the GeoJSON source in `GEOJSON_SOURCES`: `newlayer: 'newlayer.geojson',`  
3. Add roster data in `REPS`: `newlayer: { 1: { name: "...", ... }, },`  
4. Add district number lookup in `getDistrictNum()` for the new layer's field name  
5. Add a display label in `layerLabel()`: `newlayer: 'Display Name'`  
6. Upload the GeoJSON file to the repo

---

## Google Maps API Key

Embedded in `index.html`. Restricted to `publicgoodpolicy.github.io/*`, `publicgoodpolicy.org/*`, and `*.publicgoodpolicy.org/*`.

To update restrictions: Google Cloud Console → APIs & Services → Credentials.

---

## Embedding in Squarespace

\<iframe

  src="https://publicgoodpolicy.github.io/ipg-rep-finder/"

  width="100%"

  height="700px"

  style="border:none; border-radius:8px;"

  title="Find Your Representatives"\>

\</iframe\>

---

## Upcoming Maintenance

- **November 2026** — Chicago School Board expands to 20 fully elected districts. Update `schoolboard.geojson` and all school board roster entries.  
- **November 2026** — All 17 Cook County Commissioner seats up for election. Update `cookcounty` roster after results.  
- **February 2027** — Police District Council elections. Update `police` roster after results.

