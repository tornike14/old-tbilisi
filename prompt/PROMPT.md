# Build your own city as a walking sketchbook

The prompt below is the one that produced [Old Tbilisi](https://tornike14.github.io/old-tbilisi/),
rewritten for any city, with the fixes from that build folded in so you don't have to ask for them.

**What you need:** a coding agent that can run commands and open a browser on your computer
(this was built with Claude Fable 5.2 in Claude Code). A plain chat window is not enough: the
agent has to download map data, look up the real places on Google Maps, build the file and
look at its own result.

**What to expect:** Old Tbilisi took one evening: a first playable version in about an hour,
then four rounds of "this looks wrong" feedback. This rewritten prompt has not been run on a
second city yet, so expect to give feedback too.

**Before you paste it, fill in every [BRACKET]:**

- `[CITY AREA]`: the district, not the whole city. Keep it to about 1 km by 1 km. The world is
  drawn on a fixed grid, so a bigger area means blurrier walls.
- `[BBOX]`: west, south, east, north of that area in degrees. Draw the box at
  openstreetmap.org → Export and copy the four numbers.
- `[START]` and `[MAIN ROUTE]`: where the walk begins and the one street everyone should walk first.
- `[LANDMARKS]`: 10 to 17 real places inside the box.
- `[LOCAL LANGUAGE]`: the second language next to English.
- `[LOOK OF THE CITY]`: what the place is made of. Two lines are enough.

---

```
Build a walkable, hand-drawn 3D exploration game of [CITY AREA], playable in the browser,
shipped as ONE self-contained HTML file (target under 400 kB). Every pixel of the world is
drawn by WebGL2 shaders: no image textures, no 3D model files, no libraries, no game engine,
and no network requests at runtime.

THE PLACE IS REAL, NOT PROCEDURAL
The street layout, building footprints and terrain come from real map data baked into the file.
1. Write a small build script (Python or Node, standard library only) that fetches the data once
   and saves the raw downloads to a folder, so later rebuilds never fetch again:
   - OpenStreetMap, bbox [BBOX]:
     https://api.openstreetmap.org/api/0.6/map?bbox=WEST,SOUTH,EAST,NORTH
     Use a generic User-Agent with no personal details.
   - Elevation: SRTM 30 m via https://api.opentopodata.org/v1/srtm30m. Batch the points and
     respect its rate limit (100 points per request, one request per second).
   - If the area is surrounded by hills or mountains, also sample a ring of elevations around
     it and draw that as the horizon.
2. From OSM keep: building footprints (height from `height` or `building:levels`, about 3.2 m
   per level, sensible defaults by building type where both are missing), streets, paths,
   squares and steps as the walkable network, water, bridges, walls, trees, and the named
   landmarks: [LANDMARKS].
3. Quantize and compress all of it into a compact binary blob embedded in the HTML. At load,
   rasterize footprints, heights and building IDs into GPU textures and raymarch those (a
   height field plus an ID grid). Do not loop over buildings per pixel. Buildings sit on the
   terrain under them.
4. Credit "© OpenStreetMap contributors" on the title screen. The licence requires it.
Show me the building and street counts when this step is done.

THE WALK
- Start at [START]. The main route is [MAIN ROUTE], then free roaming to every landmark.
- Walk only on the real street network and squares. Buildings are solid. Slopes are real.
- Walking is brisk, about 7 to 8 metres per second, faster with Shift. Real walking pace makes
  the town feel endless.
- Click or tap anywhere to walk there along the streets. This must work across the whole map,
  and the walker must never get stuck on a corner.
- Every landmark must be reachable on foot from the start. Prove it: run the pathfinder from
  the start to each one and show me the route lengths.
- If a real transport line is mapped (cable car, funicular, ferry, tram), let me ride it.

DISCOVERIES
- Reaching a landmark stamps a sketch into a journal with 2 to 3 lines of history.
- Only write facts you can source. Fetch each landmark's Wikipedia article, and cite the
  article in a code comment above the entry. Where a place has no article, the note says only
  what the place is. No invented history.
- A banner appears every time I arrive at a landmark, not only the first time. Clicking it
  moves the camera to a view that frames the whole landmark and slides its story in from the
  right.
- Light goals instead of combat: find all landmarks, a few small errands from townsfolk, and
  the local street animals. A map drawn from the real street graph.
- Day and night, and weather. At night windows and street lamps glow and water reflects.
  Clear weather is the default. Mist and rain must never wash the buildings out.
- English by default, plus [LOCAL LANGUAGE], with a switch on the title screen and in the
  toolbar. Tell me the translation is yours so a native speaker can check it.
- A cheat sheet in the spirit of GTA: teleport by clicking the map, very fast walking, walking
  through walls, a bird's-eye view, and codes for time of day and weather.

LOOK AT THE REAL PLACE FIRST
Before you draw any landmark, look it up in a browser:
- its Wikipedia photo,
- Google Maps satellite view, for the roof and the footprint,
- Google Street View, from the street I will approach it on.
For each one write down: wall material and colour, roof shape and colour, how many towers or
domes and what shape, and the one thing a local would recognise at once.
Do the same for two or three ordinary streets, so everyday houses get the right roofs, wall
colours and balconies, and for the water and the paving.
These are references to look at only. Do not embed, trace or copy any photo or map tile.

LOOK
A traveller's sketchbook: confident ink linework over loose watercolour washes on textured
paper, paint pooling at edges, paper left white for highlights. [LOOK OF THE CITY]
Windows, balconies, arches, domes and roof tiles are simple procedural detail on the real
footprints. Characters are small, chunky and readable from a third-person, slightly top-down
camera.
- Every object must be recognisable. Street lamps are slim posts with a lantern, not thick
  columns. If I could not tell what something is, treat that as a bug.
- Walls end at their own roof line. No stray slivers of wall, no jagged towers.
- Tall monuments must be visible top to bottom from the square they stand on.

CAMERA
- I can turn and zoom it, but left alone it turns by itself when buildings hide the walker,
  and anything between the camera and the walker is cut away.
- It never jumps onto roofs.

PLATFORM
- Desktop: WASD and mouse, click to walk.
- Phone: a thumb stick and tap to walk. On a phone held upright, show a card asking to turn it
  sideways, with a button to play upright anyway (many people keep rotation locked).
- The title screen and every panel must fit a phone held sideways, about 390 px tall.
- Steady 60 fps on a laptop: adaptive internal resolution, a budget for the raymarch. Do not
  lower the resolution because the tab was hidden or throttled.

CHECK YOUR OWN WORK BEFORE YOU SHOW ME
- Open the built file in a browser yourself. Walk or teleport to every landmark, take a
  screenshot at each and look at it.
- Put each landmark screenshot next to the references you collected (Wikipedia photo, Google
  Maps satellite view, Street View) and fix colours, roof shapes and proportions. Tell me which
  landmarks you compared and what you changed.
- Test a phone size upright and sideways.
- Confirm the file makes no network request and loads no outside script or image.
- Tell me plainly what is still approximate.

Work in steps: data first (show me the counts), then rendering, then walking, then discoveries,
then the self-check tour. Tell me the final file size.
```

---

## After it works

Three follow-ups that each took one message:

- "Host this on GitHub Pages." One public repository with the file as `index.html`.
- "Record a video of it." Give the storyboard: where to start, what to ride, what to click.
- "This looks wrong: …" Say it in plain words, with a screenshot if you have one. The four
  rounds on Old Tbilisi were about things that were hard to see, a statue that did not fit in
  view, the camera hiding behind buildings, and a banner that only appeared once. All four are
  already in the prompt above.

## What it will not do

- Facades are invented. Footprints, heights and streets are real; window layouts and wall
  colours are not surveyed.
- The world is a height field, so bridges are solid underneath and passages under buildings are
  open gaps.
- SRTM is 30 m data, so cliffs and embankments come out softer than the real ones.
- Areas with poor OpenStreetMap coverage will look empty. Check your district on
  openstreetmap.org first: if buildings are not drawn there, they will not be in the game.
