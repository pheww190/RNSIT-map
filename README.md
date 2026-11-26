# RNSIT in 3D

An interactive 3D map of the **RNS Institute of Technology** campus, Channasandra, Bengaluru — built by porting [Nisarg Parashar's IIT Kanpur in 3D](https://github.com/nisarg3176/iitk-3d-map) to RNSIT with new data.

The engine, the look and every interaction are his. What changed is the data and the labels: 846 building footprints, 294 road and footpath segments and 223 named places, taken from real map data instead of being modelled by hand.

## Run it

Static site, no build step. Opening `index.html` from disk will not work (the browser blocks `campus.json` and the 3D modules that way). Serve the folder:

```bash
cd rnsit-3d
python3 -m http.server 8000
```

Then open <http://localhost:8000>. three.js and the fonts load from a CDN, so the first load needs internet; the campus data itself is local.

## What you get

- **Search anything** — buildings, departments, hostels, canteens, cafés, shops, banks, roads. Real names and the short ones people use: `canteen`, `mess`, `library`, `hostel`, `ECE`, `MBA`, `bank`. <kbd>/</kbd> or <kbd>Ctrl K</kbd>.
- **Directions from anywhere to anywhere** — From and To, or your own GPS location, with walking distance and time.
- **Walk the route** — *Start* follows you live, turn by turn; *Preview* plays the walk first.
- **Walk mode** — drop to street level and stroll the campus.
- **Day and night** — the sun, shadows and seasons follow the real time in Bengaluru; at night windows and street lamps light up.
- **2D map, angle lock, filters, sharing** — flat map, tilt lock, category filters, links to any place or route.
- **Useful spots** — banks and ATMs picked out on the map.
- **Works on a phone** — a lite mode switches itself on when the frame rate drops.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The app — Nisarg's engine, rebranded for RNSIT (title, about panel, GPS origin, home view) |
| `campus.json` | The campus data in his schema: buildings (`r` rings in metres, `t` type, `l` levels, `n` name), roads (`t`, `p`, `n`), places (`n`, `x`, `z`, `c`), plus lanes and info |
| `people.json` | Kept for compatibility; empty (the original lists IIT Kanpur faculty) |

## How the data was made

1. Building footprints, road segments and places were pulled from the **Overture Maps** release `2026-11-18.0` (public S3, queried by bounding box around the campus).
2. They were projected to local metres — `x` east, `z` south — around `12.9008° N, 77.5174° E`, which is the origin the engine expects.
3. Types were mapped into the engine's palettes: academic → `university`, hostels → `dormitory`, everything else → `yes`; OSM road classes map straight onto its road table.
4. Campus names (Boys Hostel, Girls Hostel, Canteen, Library, Auditorium, ECE Department, Main Building, MBA block, MEC/EEE, RNS International School, Shiva Temple, Canara Bank…) were checked against OpenStreetMap via Nominatim and matched to the nearest footprint.

Heights are tagged values where the source had them and estimates elsewhere (`levels × 3.3 m`).

## Port notes

- `toXZ()` GPS origin changed from Kanpur to `12.9008, 77.5174`.
- Home view centred on the campus instead of the Kelkar library.
- The IITK-only "Silver Jubilee" landmark, which falls back to hard-coded Kanpur coordinates when not found, is pushed off-map.
- About panel rewritten; day-mode text colour pinned to the theme ink so it stays readable.

## Credits

- **Nisarg Parashar** — the 3D engine, the design and every interaction, from [IIT Kanpur in 3D](https://nisarg3176.github.io/iitk-3d-map/).
- **Overture Maps Foundation** — building footprints, roads and places, release 2026-11-18.0 (ODbL / CDLA-Permissive-2.0), built largely from © OpenStreetMap contributors.
- Names cross-checked via **Nominatim**; rendering with **three.js**.

Unofficial, student-style project. Not affiliated with RNSIT.
