# Kurukkanmoola X — V24 Kerala Environment Upgrade

**What's new in V24:** the cube-and-cylinder placeholder world has been replaced by a procedural, stylized Kerala environment — an organic town with shop rows,
tiled-roof houses and compounds, bilingual shop signs, kerbed roads with markings and zebra crossings, left-hand traffic, pedestrians, street dogs and birds,
instanced Kerala vegetation, flowing-water shaders, hill mist, and **enterable tea shops** (press **E** at a chaya kada door). Gameplay systems — player, camera,
vehicles, missions, economy, multiplayer, UI — are unchanged. Read **UPGRADE-NOTES.md** for the architecture, controls, tests, extension points and limits.

---

# Kurukkanmoola X — V21 Landscape Rebuild

This revision is based on your uploaded **V20 fixed-terrain-collision** game. It implements a substantial procedural landscape and map rebuild using the two new reference images and your written master specification. The written **12,000 × 8,300 metre** dimensions take priority over the smaller dimensions printed in the reference artwork.

## Run

1. Extract the whole ZIP.
2. Install Node.js if it is not already installed.
3. Windows: double-click `START-WINDOWS.bat`. For LAN play use `START-LAN.bat`. macOS/Linux: run `sh START-MAC-LINUX.sh`, or run `npm start` in this folder.
4. Open the address printed by the launcher. The server tries port 8080 first and can use a nearby free port.
5. Enter your name and gender, then enter the game. Music, the existing guest multiplayer, messages, leaderboard, bicycle, cars, aircraft and original character/vehicle assets remain included.

The Three.js dependencies and graphics assets are bundled locally. No npm install is required to play. Browser modules require the included HTTP server; do not double-click `index.html`. Internet multiplayer still requires hosting the Node server; this ZIP is not a publicly deployed game.

## What changed

- Shared master coordinates: west/east ±6,000 m, south/north ±4,150 m, north is positive Z. The live map now displays north at the top.
- Terrain mesh and collision use identical triangle interpolation on a 25 m height grid. Town center is approximately 85 m; the main peak is about 717 m; Churuli is around 450 m; the tea-country center is around 280 m.
- All roads are generated before rendering. The hill route has connected switchbacks, slope-limited profiles, drainage/retaining details and continuous shared junction heights.
- Roads crossing water receive raised decks, supports and edge rails. Vehicles use road/bridge surface height and tilt with the slope. Bridge infrastructure is decorative apart from the driveable deck; it is not a full rigid-body simulation.
- One connected water network links the reservoir outlet, western river, town canal, backwaters and southern water, with a tributary from Kurinji Falls. Lake beds and gentle banks are carved rather than covering unmodified land with floating planes.
- Animated water normals and small vertex waves operate at all quality settings. Kurinji Falls has an approximately 87 m cascade, scrolling water streaks, spray and pool foam. Reflections use the environment map, rather than expensive per-water-level planar reflection passes.
- Metre-scale PBR ground detail, variable terrain tint, wind-driven grass, textured canopy cutouts, rooted coconut palms, rocks, hill forests, and terrain-following tea plantation rows.
- Distant forest instances plus deterministic nearby vegetation cells control rendering cost. Higher quality settings increase the detail range and grass coverage. Forest regions cover roughly a third of the map; close forest cells generate approximately 80 trees/hectare before road, water and building exclusions.
- Organic town/residential districts, farm buildings, Churuli houses, tea factory and worker housing, workshops, schools, resorts, cabins, temple complexes, campus buildings, paddy rows, bus shelters, islands, jetties and houseboats. `WORLD-AUDIT.json` records actual generated counts; requested population figures are targets, not a claim that every listed category was exactly matched.
- Six Kerala jobs you can work for money: Supermarket Cashier, Meat Cutter, Fish Seller, Shop Owner (walk-up counters — serve customers with F), plus Taxi Driver and Bus Driver (drive the parked taxi/buses between gold pickup and drop-off markers). Money is shown top-right and saved between sessions. See `JOBS-AND-WATER-UPDATE.md`.
- The river/backwater surface now has layered waves and a visible flow instead of one flat tinted plane, and Kurinji Falls is a two-tier cascade over a rock ledge with flanking cliffs, rather than a single falling sheet.
- A cached map generated from the actual terrain, water, roads and building footprints. `WORLD-MAP.png` is the generated world map; the original reference image is separately available in the reference viewer.
- Fixed the below-ground loading camera, elevation-relative jumping, and camera obstruction heights. The earlier bicycle and Mugeena aircraft remain available at the spawn.

## Controls

| Key | Action |
|---|---|
| WASD / arrows | Walk, drive, or aircraft throttle/turn |
| Shift | Run |
| E | Enter/exit a nearby vehicle or aircraft |
| Space | Jump, vehicle brake, aircraft climb |
| Ctrl | Aircraft descend |
| Mouse drag / wheel | Orbit / zoom |
| M | Map and landmark waypoints |
| V | Vehicle directory |
| F | NPC conversation / take, work, or clock out of a job |
| C | First/third-person camera |
| T | Change atmosphere |
| Esc | Pause/settings |

## Files to edit

- `src/world-layout.js`: authoritative world bounds, landmarks, road profiles, waterways, terrain and surface-height queries.
- `src/landscape.js`: terrain materials, waves, foliage instancing, vegetation detail and tea rows.
- `src/world.js`: architecture, bridges, regional props, waterfall, cached map, vehicle/NPC placement.
- `src/game.js`: controls, camera, weather, existing multiplayer UI and gameplay.
- `assets/models/manifest.json`: optional authored character, vehicle and NPC GLB replacements.
- `MASTER-MAP-SPECIFICATION.txt`: your supplied brief.

## Practical limits

This remains a procedural browser-game prototype. It does not reproduce the photographic detail of the first reference exactly. Buildings and vehicles use modeled approximations; the bundled PBR surfaces and character head are retained. Forest species are visual approximations, not individual scanned botanical assets. Boats/houseboats and jetties define the water routes visually; boat driving, jobs, farming, deliveries and interiors are not implemented gameplay systems in this revision.

Multiplayer is the uploaded V20 polling-based guest system; this landscape revision does not replace it with the different server from earlier downloads. It is intended for trusted cooperative play, with client-submitted positions/scores and session data held in memory. Weather, traffic and scenery animations are local. Do not treat the included leaderboard as a hardened competitive service.

Hardware-GPU performance and 24-player load have not been benchmarked. Use Balanced initially; choose Low on slower computers. The validation captures were made with software WebGL and are not a frame-rate claim.

Run `npm test` and `npm run check`. See `VALIDATION-V21.md` for the exact checks performed.
