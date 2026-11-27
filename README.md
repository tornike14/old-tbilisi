# Old Tbilisi — a walking sketchbook

A walkable, hand-drawn exploration game of Old Tbilisi in one self-contained HTML file.

**Play it: https://tornike14.github.io/old-tbilisi/**

The street layout, building footprints, river, fortress walls and terrain are real map data baked
into the page. Everything on screen is drawn by WebGL2 shaders from that data. Nothing is loaded at
runtime: no images, no models, no network.

## Play

| | |
|---|---|
| W A S D / arrows | walk (Shift to hurry) |
| Click / tap | walk to that spot along the street network |
| Drag, wheel / pinch, Q E | turn and zoom the view |
| F | talk, ride the cable car |
| V, or tap a banner | sight view: the camera frames the landmark and its story slides in |
| J, M | sketchbook, map |
| C | cheat codes |
| Phone | left thumb is a stick, tap to walk, drag on the right to look |

Goals: sketch all 17 landmarks, find 8 street cats and 5 ear-tagged dogs, finish 6 errands.
English is the default language; the ქართული / English button switches to Georgian and back.

## Sources

- Map data © OpenStreetMap contributors, ODbL.
- Terrain: NASA SRTM 30 m, fetched through api.opentopodata.org.
- Journal notes are written from English Wikipedia; each entry's source article is cited in a
  comment above it in the page source.
