# 霧界繪行者：Roguelike Hybrid Redesign

## Design thesis

The world is persistent, physical and walkable. The roguelike lives inside expedition spaces.

This avoids two common failures:
- turning a beautiful overworld into an abstract node map
- making procedural generation feel like endless anonymous corridors

The player should remember places: the flooded bunker beneath a marsh, the tunnel that exits behind a mountain chain, the sea cave that only opens at low tide. Each expedition can vary without destroying the identity of that place.

## Persistent layer: the restored world

The overworld contains authored regions with physical entrances and distant landmarks. Fog hides geography instead of simply disabling UI buttons.

Persistent progression includes:
- revealed geography
- discovered entrances and shortcuts
- faction relationships
- recovered cartography
- permanent world changes
- learned enemy information
- character memories and scars
- traversal tools

If the player fails an expedition, these remain.

## Run layer: expeditions

Entering a cave, bunker, tunnel, sea cave, ruin or sky facility starts an expedition with a deterministic seed.

A run contains 3-8 authored chunks assembled into a walkable floor. Each chunk has sockets and gameplay tags.

Example: Echo Cave
- entrance gallery
- singing crystal chamber
- flooded fork
- collapsed rail tunnel
- smuggler shrine
- cartographer cache
- deep predator nest
- alternate exit

The run chooses a subset and arrangement. The cave still feels like Echo Cave.

## Expedition pressure

No generic energy timer. Pressure comes from expedition-specific systems:
- lantern oil / visibility
- rising tide
- oxygen in submerged ruins
- bunker alarm level
- structural instability
- enemy pursuit
- corruption / memory distortion

Pressure forces choices without requiring real-time monetization mechanics.

## Risk and extraction

A player can retreat from most expeditions.

Loot categories:
- secured knowledge: map notes, discovered shortcuts, enemy observations; kept on retreat/death
- carried goods: materials, consumables, trade items; lost or partially lost if defeated
- bound relics: rare rule-changing equipment that becomes secure after reaching a safe point

This creates tension without making failure erase hours of exploration.

## Loot philosophy

Loot should create new verbs.

Examples:
- Folding Bridge: cross one-tile gaps and create temporary tactical bridges
- Tide Bell: change water level once per expedition
- Mothfire Lamp: reveal hidden doors but attracts nocturnal enemies
- Wrong-Way Compass: redirects one enemy reinforcement entrance
- Windcloth: bends a projectile or opens wind routes on the overworld
- Resonance Spike: marks a cave wall; if struck from the opposite side later, creates a permanent tunnel

Numeric stats exist, but are not the reason an item is exciting.

## Tactical combat

Combat is turn-based tactical positioning with interleaved initiative.

Battle goals vary:
- steal a map case and extract
- hold a survey point
- redirect water
- escape a collapsing chamber
- escort a cartographer
- lure a monster into another faction
- interrupt a ritual
- survive until a moving sky-island passes overhead

Not every battle requires killing every enemy.

## 2D / 3D hybrid presentation

### 2D remains responsible for
- overworld exploration
- dungeon movement
- collision and traversal
- normal NPC interaction
- inventory, maps and world state

### Three.js/WebGPU burst layer handles
- ability targeting and dramatic cast previews
- spell impacts and volumetric effects
- boss introductions
- giant GLB machinery and creatures
- special battle arenas
- short environmental cinematics

This layer is optional per encounter and can fall back to 2D effects.

## VFX module architecture

Inspired by LinearAbiltyCastingThreeJS:

```text
vfx/
  core/
    VfxStage
    CameraRig
    Time
  abilities/
    Ability
    FrostLine
    StormLine
    CinderZone
  targeting/
    LineIndicator
    ZoneIndicator
  effects/
    Decal
    Burst
    CameraShake
    ScreenFlash
  particles/
    Pool
    Emitter
  config/
    presets
```

Every ability has phases:

```text
arm -> commit -> travel -> impact -> linger -> fade
```

A gameplay event drives the effect. The effect never decides damage or tactical legality.

## WebGPU editor role

`three-webgpu-editor` is not embedded as the runtime game engine. It is an authoring tool/reference for:
- placing GLB assets
- adjusting light and sky
- assembling boss arenas
- exporting scene configuration
- previewing WebGPU materials and effects

Runtime should load a compact exported scene/preset rather than shipping the entire editor UI.

## First playable redesign milestone: v0.4

1. Persistent island overworld remains walkable.
2. Echo Cave becomes a seeded expedition instead of one fixed room.
3. Three chunk types are implemented: traversal, event, cache.
4. One pressure meter: resonance instability.
5. Player can voluntarily extract from the entrance.
6. Two relics alter traversal rules.
7. One tactical encounter is triggered inside the expedition.
8. Winning can drop a map fragment.
9. A separate Three.js VFX lab demonstrates line cast and zone cast effects.
10. Production asset requests are delegated to `agent-sprite-forge` through the repository AGENTS.md workflow.

## Self-review gates

Before v0.4 is promoted to the main loop, verify:
- seed replay produces the same floor
- all generated floors have a valid path from entrance to objective and extraction
- the player understands what can be lost before entering
- at least two meaningful route choices exist
- relics change decisions rather than only numbers
- mobile controls can complete the run without keyboard/mouse
- disabling Three.js does not make gameplay impossible
