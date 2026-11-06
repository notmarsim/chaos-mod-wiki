# Chaos Forge geometry audit

## Authoritative source located

The existing file is named `chaos_monolith.schem`, rather than the `chaos_monolith_worldedit.schem` spelling in the master prompt:

`C:/Users/mhrme/curseforge/minecraft/Instances/ChaosMod Dev Test/config/worldedit/schematics/chaos_monolith.schem`

Its dimensions, palette, all component counts and every specified functional coordinate match the requested design exactly. No replacement geometry was invented.

- Sponge schematic version: 2.
- Minecraft DataVersion: 3465.
- Dimensions: 35 × 25 × 35.
- Physical blocks: 4,085.
- Air cells: 26,540.
- Source SHA-256: `c9e475ea6afaf3355486ed4984e1063a3dfbfd73e49bb316d7e80fe8172b2200`.
- Metadata world-edit offsets and schematic offset are zero.

## Corrected component mapping

| Source placeholder | Definitive component | Final amount |
| --- | --- | ---: |
| `minecraft:deepslate_tiles` | `chaos_forge_foundation` | 1,225 |
| `minecraft:obsidian` | `chaos_forge_dimensional_anchor` | 978 |
| `minecraft:polished_deepslate` | `chaos_forge_support_frame` | 728 |
| `minecraft:polished_blackstone_bricks` | `chaos_forge_casing` | 723 |
| `minecraft:magenta_concrete` | `chaos_forge_heat_coil` | 196 |
| `minecraft:purple_concrete` | `chaos_forge_energy_conduit` | 105 |
| `minecraft:tinted_glass` | `chaos_forge_glass` | 97 |
| `minecraft:cyan_stained_glass` | `chaos_forge_portal_glass` | 25 |
| `minecraft:lime_concrete` | `chaos_forge_energy_input_port` | 4 |
| `minecraft:blue_concrete` | `chaos_forge_item_input_port` | 1 |
| `minecraft:orange_concrete` | `chaos_forge_item_output_port` | 1 |
| `minecraft:red_concrete` | `chaos_forge_terminal` | 1 |
| `minecraft:beacon` | `chaos_forge_core` | 1 |

The source has the exact two cells described by the prompt: the inaccessible Energy Port at `(17,2,3)` and Casing at `(13,2,2)`. The definitive asset must replace the first with Anchor and the second with Energy Input Port. This retains 4,085 physical blocks.

## Functional coordinates and accessible faces

Coordinates are relative to the unrotated schematic origin. The specified facing is the external cable/UI-facing direction.

| Component | Position | Accessible face | Adjacent connection cell |
| --- | --- | --- | --- |
| Terminal | `(17,2,2)` | North | `(17,2,1)` |
| Item Input Port | `(15,2,2)` | North | `(15,2,1)` |
| Item Output Port | `(19,2,2)` | North | `(19,2,1)` |
| Front Energy Input Port | `(13,2,2)` | North | `(13,2,1)` |
| Left Energy Input Port | `(2,2,17)` | West | `(1,2,17)` |
| Right Energy Input Port | `(32,2,17)` | East | `(33,2,17)` |
| Rear Energy Input Port | `(17,2,32)` | South | `(17,2,33)` |
| Core | `(17,24,17)` | — | — |

All seven interface connection cells are air in the source, with a clear path to the outside. Under rotation `r`, positions transform by rotating their displacement from the terminal, and connection normals transform by `r.rotate(direction)`.

## Validation architecture

The geometry should be generated from the corrected schematic into a compact data-driven template. Validate the 4,085 physical positions incrementally with a bounded budget, caching the successful result and auditing only slices afterward. Revalidate on relevant component changes. Check loaded chunks without requesting or forcing chunk loads; suspend processing when any needed chunk is unavailable.

Do **not** classify all 26,540 schematic air cells as required empty cells. The interfaces are inset from the 35 × 35 boundary, so their legitimate adjacent energy/item cable cells are inside that bounding box. Reserving every schematic air cell would make connecting cables invalidate the machine. Instead, structural cells that are air report Incomplete; structural cells occupied by an incorrect block report Obstructed. The prompt does not mandate additional internal required-air cells.

Only the Terminal and functional ports require BlockEntities. The 4,078 other structural/decorative components have no individual inventory or ticking logic.
