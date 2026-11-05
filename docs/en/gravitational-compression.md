# Gravitational Compression Terminal

The Nova-tier Gravitational Compression Terminal packs simple materials into a single **Compressed Matter** item and releases their contents through ordinary item inventories. AE2, Refined Storage and transporters handle that item normally; they do not treat it as its original ingredients.

## Build the chamber

Build the fixed **13 × 11 × 13** mold. The clean reference is available as `chaosmod:gravitational_compression_chamber` through a Structure Block. The original world template is archived for comparison; no foreign placeholder blocks are required.

- Foundation Y0: 168 Gravitational Compression Casings and one Compression Core at X6, Y0, Z6.
- Roof Y10: 169 Casings.
- Four corner pillars: 36 Gravitational Frames, spanning Y1–Y9.
- Walls: 396 Gravitational Shielded Glass before replacing functional positions.
- Containment rings: 16 Coils at Y1 and another 16 at Y7. In each ring, Z3/9 uses X5–7, Z4/8 uses X4/8, and Z5–7 uses X3/9.
- Emitter columns: X/Z4/4, 4/8, 8/4 and 8/8, each spanning Y2–Y6: 20 Field Emitters.
- Keep all other interior cells empty. The visual singularity at X6, Y5, Z6 is **air**.

Replace any non-edge exterior face cell with exactly one Terminal, at least one Item Input Port, at least one Item Output Port and at least one Energy Port. Roof and foundation placements are allowed. The foundation Core cannot be replaced. Ports automatically face outward and multiple ports share the same Core resources.

The casing uses the unique ID `gravitational_compression_casing`, preserving the existing Event Horizon Forge's `gravitational_casing` block and recipe.

## Compress material

Select **Compression** and Level I or II while stopped. Feed eligible materials into an input port or the terminal deposit slot. The Core stores one 64-bit count per material instead of millions of stacks. The selected material is preferred; other available materials can also process automatically.

| Level | Original units | Time | Consumption | Total energy |
| --- | ---: | ---: | ---: | ---: |
| I | 1,000,000 | 60 s | 800,000 FE/t | 960 MFE |
| II | 1,000,000,000 | 600 s | 8,000,000 FE/t | 96 GFE |

The machine consumes material only when the completed product can be safely stored. Missing FE, incomplete structure, unloaded chunks or full output suspend work. Stop preserves progress; changing mode/level/material while stopped cancels paid progress and keeps the ingredients.

## Decompress and extract partially

Stop and switch to **Decompression**, then feed canonical Compressed Matter into an input port. Decompression takes one quarter of the corresponding compression time and half its FE/t. A completed operation credits its original material count to the virtual stock. The Core releases at most one stack per tick into the nine physical product slots, so transporters can extract any amount without creating giant item stacks or world entities. Unextracted material remains persistent virtual stock.

Changing mode never duplicates balances. The Core drop retains virtual counts, products, stabilizers, paid progress and energy. Keep the dropped Core safe when moving the machine.

## Stability, eligibility and automation

The chamber uses Nova machine Stability and three dedicated stabilizer slots. Flux Crystals, Empowered Flux Crystals, Reality Condensate and Creative Stabilizers use the existing recovery rules. Running at zero stability causes containment failure. Black Holes have no growth or mass mechanic and are not needed by the initial process.

Eligible materials are controlled by `chaosmod:compressible` and vetoed by `chaosmod:compression_blacklist` item tags. Default categories include ingots, nuggets, gems, dusts, raw materials and storage blocks. Tools, damaged items, individually tagged stacks, energy containers, fluid containers and inventories are rejected.

Input and output ports expose Forge `IItemHandler`; Energy Ports expose Forge Energy with a shared 32 MFE/t input budget and 20 GFE buffer. Multiple ports do not accelerate the internal process. Both fixed compression levels appear in JEI for every eligible material.

Settings are centralized in `chaosmod-compression.toml`. Changing unit ratios changes the meaning of existing level-tagged Compressed Matter; back up the world first. Counts never use floating-point arithmetic, and excess input is rejected before long overflow.

## Existing structures and configurable item ports

Both **Gravitational Casing** (`gravitational_casing`) from existing builds and **Gravitational Compression Casing** are valid in the foundation and roof. The actual player's reference was rejected because its 337 earlier casings were not recognized. All frame, coil, emitter, shell, and empty-space rules remain enforced.

Craft **Gravitational Item Ports** and use the Configurator to select input or output. The front indicator changes with the mode. Old input/output blocks remain supported and configurable; no replacement is required. Install at least one input-mode port and one output-mode port.

Opening the terminal explicitly starts server-side validation. Hover the diagnostic status for expected/found block IDs and the world/local coordinate of the first failure. One bounded diagnostic is written to the log for each requested formation, with automatic retries remaining silent.

## Viewing stored materials

Deposited ingredients move into the Core's virtual inventory on the next server tick. They remain visible in the terminal's **Stored materials** browser: each row shows original units and compressed items, with exact per-level amounts in its tooltip. Page controls reveal every stored type. Clicking a row while stopped selects that material for processing. Each viewer has an independent page; the selected processing material remains a machine setting.

A manual deposit opens the page containing that material. Cable insertions keep the page being viewed. Physical output stacks and stabilizers continue using ordinary inventory slots. Opening an already formed terminal no longer displays a formation success message.
