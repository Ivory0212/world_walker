# World Walker Codex Instructions

## Product direction

This is a handcrafted-feeling 2D pixel-art exploration RPG with roguelike subspaces. The overworld must remain a continuous walkable map. Never replace exploration with abstract node selection.

Core loop:
1. Walk the overworld and discover physical entrances such as caves, bunkers, tunnels, sea caves, ruins and sky facilities.
2. Enter a subspace. Its internal layout, encounters, hazards, treasure and exits may be generated from a deterministic run seed.
3. Tactical encounters reward rule-changing gear, maps and information rather than pure stat upgrades.
4. Survive or retreat with discoveries. Losing a run may cost carried loot, but permanent cartography and world knowledge remain.
5. Decode map fragments in safe locations to physically clear fog and restore new overworld geography.

## Art pipeline

The repository is intended to be used with the installed Codex skill/agent `agent-sprite-forge` when available.

When new production art is needed, use `agent-sprite-forge` instead of drawing placeholder assets by hand unless the task is only a temporary debug visualization.

Required art style:
- detailed top-down 16/32-bit-style pixel art
- dense natural environments, readable terrain height, soft organic coastlines
- colorful but not childish
- coherent lighting and palette across tiles
- characters readable at gameplay scale
- no star-rarity visual language
- no generic mobile idle-game UI

For every generated asset, also update `assets/manifest.json` with:
- id
- file path
- type (tileset / prop / character / monster / ui / effect / portrait)
- native dimensions
- gameplay collision footprint where relevant
- palette / biome tags
- source workflow (`agent-sprite-forge` or other)
- commercial-use/license note

Sprite requirements:
- player/NPC: 4 directions minimum; idle + walk; combat actors also need attack/hurt/down
- monsters: idle + move + attack + hurt + down
- tiles: seamless edges and transition tiles, not isolated single-texture squares
- entrances must visually read as part of the terrain, never as floating buttons

## 2D / 3D hybrid rule

The game is 2D-first. Three.js/WebGPU is a burst layer, not the primary world renderer.

Use 3D for:
- tactical battle VFX
- boss or giant-creature presentation
- GLB relics / machinery / special ruins
- short cinematic environmental events
- previewing/exporting 3D content

Do not convert the overworld into free-camera 3D.

## VFX architecture

Take architectural inspiration from `achrefelouafi/LinearAbiltyCastingThreeJS` (MIT):
- central live settings/preset object
- Ability base class
- line and zone targeting primitives
- pooled particles
- separate phases: arm -> cast -> travel -> impact -> linger -> fade
- camera shake / flash / decals as independent effects

Do not copy bundled third-party FBX/HDR assets into this repository unless their individual licenses are verified.

For heavy particle effects, prefer WebGPU when available but always provide a WebGL2/2D fallback on mobile.

## Roguelike generation rules

Generation must preserve authored identity. Randomness chooses composition, not visual quality.

Each subspace is assembled from authored chunks with contracts:
- entrance sockets
- exit sockets
- traversal tags
- hazard tags
- encounter budget
- secret budget
- landmark slot

A generated floor is invalid unless:
- entrance can reach at least one exit
- critical objective is reachable without a soft lock
- required key/ability can be obtained before its gate
- at least one retreat route remains unless the encounter explicitly locks the room
- no mandatory path depends on random cosmetic props

Use deterministic seeds and store the run seed in save data for reproducibility.

## Progression rules

Avoid:
- star rarity
- endless item level inflation
- idle rewards
- mandatory daily login
- loot whose only purpose is +5% damage

Prefer:
- equipment that changes movement or targeting rules
- map tools that open new traversal options
- character memories/traits earned by actual actions
- mutually exclusive loadout choices
- knowledge as progression

## Development loop

For every meaningful feature:
1. define the player decision it adds
2. implement the smallest playable version
3. test reachability and mobile controls
4. identify one way it can become repetitive or exploitable
5. revise before expanding content

Do not call a feature complete just because it renders.