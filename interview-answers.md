# Scrap & Rust — interview record (auto-answered with recommended options)

User sent the brief with no extra instructions and "Start here", so per the brief's
rule ("If I answer 'go'/'you pick', take the recommended ideas"), the recommended
option is taken for every interview question. Recorded here as the design source.

## Q1 — What is the mashup, in one sentence?
**Chosen (recommended):** You play *in* Scrap Mechanic (host). Rust brings its real
survival rules into it: a day/night raid cycle, scrap-farming that decays your base,
and Rust's own weapon **definitions** (names/stats read from the player's Rust
install data at runtime) you craft and fire at raiders. Scrap Mechanic contributes
its building, bearings, connectors and contraptions; Rust contributes raids,
loot tiers and decay.

## Q2 — Which game is the player in, and what does each bring?
- Primary/host: **Scrap Mechanic** (`custom-scrap-mechanic`, installed game; Melty
  launches it; folder `{game}`). Player builds/defends with SM parts and physics.
- Companion: **Rust** (`rust`, role `companion`). Melty verifies the player has it
  and passes `{game:rust}`; the mod reads Rust's real files at startup/runtime:
  - item/weapon stats + names from Rust's data (OxgmData/item DBs where present,
    else parsed from the game's managed assemblies' string tables — read-only),
  - Rust's sound effects (*.ogg in Rust's `assets/bundled/audio`) for gunfire/raids,
  - Rust's HUD color palette / raid-cycle timings mirrored from public server
    conventions documented in Rust's own config defaults.
  Nothing from Rust is copied into the package or into Scrap Mechanic's install;
  every player uses their own copy.

## Q3 — First minute of play
Spawn in a small Scrap Mechanic survival map; a tutorial beacon shows one Rust-style
loot crate (built from SM blocks, but its contents table comes from Rust's item
sheet). Loot a Rock → farm scrap → first nightfall triggers a raid wave.

## Q4 — Solo or multiplayer?
**Chosen (recommended): solo first.** Multiplayer would need Rust-style hosting
inside Scrap Mechanic's own co-op, which we do not invent. Listing states single
player. (Can be extended later.)

## License / remix
**Chosen (recommended):** MIT for our code; remix allowed = yes. Game content stays
with each game (never shipped). Credits: original mod authors only (user + agent);
no third-party code bundled unless license-checked.

## Constraints learned (Melty side)
- Scrap Mechanic: not in catalog; custom- slug usable as host; no loader supplied;
  inspect install to find mod folders/exe. Steam AppID 387990.
- Rust: unity, no one-click loader, client mods blocked by anti-cheat → we NEVER
  inject into Rust. We only read Rust's data files from disk while SM runs. This
  keeps players' accounts safe and is allowed (companion role = read files).
- One-click rule: everything must launch with Play once. Our route: Melty installs
  our program into `{managed}`… actually mode `installed` into SM host folder,
  launching SM exe via kind "game"/exe with our SM Lua mod pre-placed in SM's
  official mod folder (SM supports Lua-based mods natively — no external loader).
