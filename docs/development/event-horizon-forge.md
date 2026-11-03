# Event Horizon Forge

## Structural blueprint

`art/structures/event_horizon_reference.nbt` preserves the user-provided structure. Run `python tools/import_horizon_template.py` to regenerate the portable, sparse validator blueprint and the Chaos-only placeable structure. Runtime validation does not depend on Mekanism or on an absolute user save path.

The reference is 7×62×60 and contains 4,210 solid blocks: 2,370 Coal Blocks, 18 Jungle Wood, 1,818 Refined Obsidian and four Supercharged Coils. Coal and wood map to Gravitational Casing; obsidian maps to Darklight Reinforced Casing; coils map to Gravitational Projectors.

Template coordinates are measured from its minimum corner. The Control Core is at `(3, 1, 30)`, at the center of the pedestal spanning `(2, 0, 29)` through `(4, 2, 31)`. An additional Gravitational Casing at `(3, 3, 30)` bridges the original one-block air gap to the ring. This is the only addition to its structural silhouette.

The singularity center is `(3, 32, 30)`. Projectors are at `(3, 7, 30)` facing Up, `(3, 32, 5)` facing South, `(3, 32, 55)` facing North and `(3, 57, 30)` facing Down. The Core's East orientation matches the source; South, West and North rotate the full blueprint by 90°, 180° and 270°. Face the Core correctly before enclosing it.

Only the center of each of the four side faces and the bottom face accepts a Terminal or Port. These five usable positions can be freely permuted between one Terminal, one Energy Port, one Fluid Port and two Item Ports (Input and Output). The top face center `(3, 2, 30)` must be Gravitational Casing, continuing through the connector at `(3, 3, 30)` into the ring. All 20 shell edge/corner positions remain casing; they cannot hold a Terminal or Port. The example blueprint puts the Fluid Port on the bottom face, but the player may exchange it with any of the other four controls. The Configurator switches the single Item Port block between green Input and orange Output. Ports share one canonical inventory, tank and energy buffer. Port blocks are not accepted elsewhere on the ring. Only the central 5×5×5 region must remain empty; other unrelated interior air is not scanned.

For an operator-only construction reference, use `/place template chaosmod:event_horizon_forge` in a suitable clear area. This is an explicit placement command; the structure has no natural generation. The example pedestal already includes the required ports and connector.

## Processing

| Mode, Safe intensity | Input | Fluid | Duration | FE/t | Output |
| --- | --- | --- | --- | ---: | --- |
| Singularity Condensation | 16 Antimatter | 16,000 mB Darklight Liquid | 2,400 ticks / 120 seconds | 1,000,000 | 1 Black Hole |
| Event Horizon Transmutation | 1 Darklight Bar | 250 mB Darklight Liquid | 200 ticks / 10 seconds | 500,000 | 1 Nova Fragment |

Transmutation requires an installed Black Hole and consumes one mass unit per Safe operation. A fresh Black Hole has 1,024 units by default. Install moves one item from input or output into the protected internal slot; Remove moves that same item back to output while idle. Its `HorizonMass` and `HorizonMassCapacity` NBT store milli-units, so removal, reinsertion and saving never restore spent mass. Depleted singularities are removed. Automation may install from the input inventory in Transmutation mode, but never silently claims an output Black Hole that could also be exported.

Balanced intensity halves processing time, uses 2.8× FE/t and 1.25× mass per operation. Overdriven quarters processing time, uses 8× FE/t and 2× mass. Mode/intensity changes are rejected while a reserved job exists. Stop suspends it. Starting a job atomically reserves its items and fluid; completion commits the output and mass cost once. Missing FE or full outputs pause progress without consuming additional resources. Singularity Mass and Darklight machine Stability are separate resources.

The Control Core holds nine inputs, nine outputs, three stabilizer slots, one protected singularity slot, an 8 GFE buffer and a 128,000 mB tank. Its default shared energy input is 16 MFE/t across all Energy Ports. Input Item Ports also accept existing Flux Crystal, Empowered Flux Crystal and Reality Condensate stabilizers in their dedicated slots. Existing Stability mechanics are reused; no separate calibration system is added.

Construction recipes and both process recipes originate in `HorizonAssets` and are recreated by `runData`. Process data packs can override the two `chaosmod:event_horizon` recipes. Server tuning is in `serverconfig/chaosmod-event-horizon.toml`.

## Stellar Liquefier extension

The existing Stellar Bar → Stellar Liquid recipe retains its original values. Darklight Bars produce 1,000 mB Darklight Liquid in 2,400 ticks at 64,000 FE/t by default. The additional settings live under `liquefier.darklight` in the existing Stellar energy server configuration. Both recipes use the same existing input and output tank. Drain the previous liquid before switching; incompatible liquids never mix. The saved job records its output liquid separately from the next input item. Stellar Reactors continue accepting only Stellar Liquid.

Darklight Liquid has source/flowing variants, animated textures, a bucket, fluid tags, Forge fluid capabilities and JEI support. Existing Chaos Fluid Cables can transfer it from the Liquefier's output to the Forge's Fluid Input Port.

## Validation, persistence and rendering

Initial validation visits 256 required positions per tick by default; cached structures audit 32 positions per tick. Block edits and relevant chunk events invalidate cached state. Unloaded required chunks suspend processing and ports without forcing chunk loads. Diagnostics show validation progress, the error category and the affected coordinates. Normal Core drops preserve the inventory, paid job, fluid, energy, configuration and singularity item in its BlockEntityTag, rather than dropping a second copy of its contents.

The client draws a bounded singularity mesh, accretion rings, four containment beams and a completion pulse. This is a visual effect with no terrain destruction, entity attraction or permanent central Black Hole block. Server processing controls its state. UI and item tooltips expose remaining mass; JEI shows real recipes and the singularity's catalytic role.

In-game visual inspection remains deferred at the user's request. Automated validation covers four orientations, full processing, mass and escrow persistence, full outputs, energy loss, port replacement and the existing Stellar recipe.

## Editable Blockbench models

All nine runtime model variants are authored through the installed Blockbench MCP plugin. Native editable projects, MCP previews and the authoring manifest are in `art/models/event_horizon/`. The shared Item Port has separate input/output indicator variants, with the same geometry.

With Blockbench and its MCP plugin running on `localhost:3000`, run `python tools/blockbench/build_horizon_machines.py` (or the compatibility entry point `tools/build_horizon_models.py`) to regenerate the models and native 64×64 texture atlases. The script creates new projects, uses Blockbench cubes, native texture painting and export codecs, and preserves existing projects. Runtime JSON meshes fit within 0–16 block coordinates and cull only faces on the block boundary.

Terminal and port panels automatically face away from the Control Core, including the bottom face, regardless of construction order. The top face stays structural.
