# Chaos Forge — construction assets and recipe budget

The thirteen components use native Minecraft cuboid JSON models and newly authored pixel textures. No Blockbench MCP connection was used. `tools/BuildChaosForgeAssets.java` reproduces the meshes and textures; the existing headless model previewer parsed every model through Minecraft's native `BlockModel` parser.

## Material palette and design

The artwork samples the registered ingredient textures rather than assigning an unrelated tier colour:

- Chaos Bar dominant saturated red: **RGB 143, 0, 0**.
- Chaos Circuit dominant saturated red: **RGB 211, 0, 0**.
- Graphite insulation and worked-steel rails follow the Event Horizon Forge, Nova Reprocessor and existing industrial casing language.
- Highlights are restrained warm white; no purple, yellow or blue is introduced into the Chaos structural palette.

| Part | Visual treatment |
| --- | --- |
| Foundation | Heavy sealed plate with horizontal structural seams and captive fasteners. |
| Dimensional Anchor | Black containment mass with narrow inset red channels. |
| Support Frame | Closed diagonal bracing; no unintentional holes through the model. |
| Casing | Symmetric reinforced armor and restrained status pins. |
| Heat Coil | Recessed winding bed, individual Chaos conductors and ceramic clamps. |
| Energy Conduit | Insulated internal bus with narrow red conductors and steel keepers. |
| Glass | Thin graphite frame, small red corner clips, nearly clear centre. |
| Portal Glass | The same clean frame with faint red dimensional etching. |
| Energy Input | Two visibly separate high-current contacts and isolation spacers. |
| Item Input / Output | Roller couplings and opposite directional arrows. |
| Terminal | Recessed forging monitor, control keys and status strip. |
| Core | Containment lens, crystalline centre and magnetic focus rails. |

All opaque models have a sealed full-block silhouette. Detail meshes remain inside `[0,16]` and omit overlapping outer chassis faces. The front of functional models points north; datagen creates horizontal variants and inventory models. The four Energy Ports must face their respective accessible exterior sides, with the whole blueprint then rotated normally.

Resources are maintained under `src/main/resources/assets/chaosmod/models/block/chaos_forge_*.json` and `textures/block/chaos_forge/`. These are source assets, not files authored only into generated output. `ForgeAssets.models` regenerates blockstates and item model links.

## Construction recipes

All recipes are registered by `ForgeRecipes.generate` as Void Station recipes. The patterns are centred five-by-five arrangements in the existing nine-by-nine recipe format, retaining ordinary recipe shifting support. No new recipe serializer or machine is necessary for constructing the first Forge.

Abbreviations: F = Foundation, A = Anchor, S = Support Frame, C = Forge Casing.

| Output per craft | Ingredients per craft |
| --- | --- |
| Foundation ×8 | Industrial Casing ×8; Chaos Bar ×1. |
| Anchor ×4 | F ×4; Chaos Bar ×2; Chaos Rod ×4; Chaos Fragment ×1. |
| Support Frame ×8 | F ×4; Chaos Rod ×4; Chaos Bar ×1. |
| Casing ×8 | F ×8; Chaos Bar ×1. |
| Heat Coil ×4 | C ×4; Chaos Wire ×8; Chaos Bar ×2; Chaos Rod ×2. |
| Energy Conduit ×8 | S ×4; Chaos Wire ×4; Chaos Cable ×1. |
| Glass ×8 | C ×4; any Forge-tagged Glass ×8; Chaos Wire ×1. |
| Portal Glass ×4 | Forge Glass ×4; Chaos Wire ×4; Chaos Orb ×1. |
| Energy Input Port ×1 | C ×14; Chaos Wire ×6; Chaos Capacitor ×4; Chaos Cable ×1. |
| Item Input Port ×1 | C ×14; Chaos Wire ×6; Chaos Transporter Cable ×4; Hopper ×1. |
| Item Output Port ×1 | C ×14; Chaos Wire ×6; Chaos Transporter Cable ×4; Dropper ×1. |
| Terminal ×1 | C ×14; Chaos Wire ×6; Chaos Capacitor ×2; Chaos Circuit ×2; Chaos Orb ×1. |
| Core ×1 | A ×12; Chaos Circuit ×4; Chaos Capacitor ×4; Chaos Wire ×4; Chaos Orb ×1. |

Foundation and Casing are deliberately bulk recipes because they occur thousands of times once intermediate parts are included. Individual Anchors use Chaos Fragments rather than a whole expensive Chaos Orb each; concentrated Orb/Circuit/Capacitor costs belong to the few specialised components. This preserves substantial Chaos progression cost without making the 978 structural Anchors require 978 high-complexity control components.

### Complete structure budget

For the corrected 4,085-block schematic, including intermediate component crafts and whole-batch rounding, the direct pre-Forge material bill is:

| Material | Required |
| --- | ---: |
| Industrial Casing | 3,704 |
| Chaos Bar | 1,291 |
| Chaos Rod | 1,482 |
| Chaos Wire | 538 |
| Chaos Fragment | 248 |
| Chaos Orb | 9 |
| Chaos Capacitor | 22 |
| Chaos Circuit | 6 |
| Chaos Cable | 18 |
| Chaos Transporter Cable | 8 |
| Any Forge-tagged Glass | 128 |
| Hopper | 1 |
| Dropper | 1 |

This table does not recursively expand existing Chaos Bar/Circuit/etc recipes. Those recipes remain unchanged. Batch leftovers are seven Foundation, two Anchor, seven Casing, seven Conduit, three Glass and three Portal Glass.

The eleven inherited Chaos Anvil outputs are equipment: Sword, Pickaxe, Axe, Shovel, Hoe, Helmet, Chestplate, Leggings, Boots, Bow and Shield. None is an ingredient in the thirteen component recipes. Chaos Bars, Rods, Wires, Fragments, Orbs, Capacitors and Circuits are already obtainable through the existing pre-Forge systems, so construction introduces no Forge-to-itself dependency.

## Verification

- All thirteen five-by-five patterns and key maps were checked for dimensions, missing/unused keys and dependency cycles.
- All thirteen native models parsed successfully; 117 cuboids have valid bounded geometry and UV coordinates.
- Software preview: `build/reports/chaos-forge/models.png`.
- Exact component counts and raw bill: `build/reports/chaos-forge/recipe-bill.json`.
- No Minecraft window, OpenGL context or Gradle process was started by the asset task.

## Post-craft visual phases

The inherited recipe transaction has zero processing ticks. `ForgeRenderer` therefore renders a **cosmetic 60-tick sequence after completion**, without delaying output, reserving items or charging more FE:

1. **Energy Convergence, ticks 0–20:** actual tower caps activate sequentially; red conductor accents and the central focus gain intensity.
2. **Chaos Compression, ticks 20–40:** the core and counter-rotating containment lenses contract, while coil and portal-window accents intensify.
3. **Reality Forging, ticks 40–60:** a brief expanding completion pulse follows the core's peak, then the effects fade back to idle.

The four beam sources are derived from the four disconnected Anchor components and their actual isolated upper Heat Coils at local `(17,13,4)`, `(4,13,17)`, `(30,13,17)` and `(17,13,30)`. Every source exits its real upper face with 0.015 blocks of clearance. All four rays reach the focus above the Core without crossing any blueprint block.

`ForgeVisualGeometry` caches the blueprint map, connected components and exposed surfaces once. `ForgeRenderer` uses one terminal renderer, bounded representative surface accents, the shared opaque/additive mesh helpers, three distance-based detail levels and a 128-block range. There are no decorative BlockEntities, world particles, looping packet emissions or per-frame structural scans. Actual inventory and energy transactions are entirely independent of rendering.

The focus is derived from the Core at `(17,24,17)`, positioned at `(17.5,26.5,17.5)` in continuous local coordinates. Its reaction body and tilted lenses fit in the clear air above the Core, and the full completion envelope fits the controller's additional five-block render height. The exact local-to-world transform is verified in all four rotations by `ForgeVisualGameTests`.
