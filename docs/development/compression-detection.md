# Compression Chamber detection repair

The updated player-built world file `gravitational_compression_chamber.nbt` matches the counts and geometry described as the definitive `(2)` reference. No separate `(2)` file was present in the supplied locations. Its exact block identities were checked at all 1,859 cells.

Root cause: all 337 foundation/roof blocks use `chaosmod:gravitational_casing`, the earlier Event Horizon Forge casing. `CompressionStructure.part` only recognized `CompressionBlock`, so those existing blocks were rejected as missing components, starting at NBT-local `(0,0,0)`. Terminal discovery itself resolves the foundation Core from arbitrary shell positions; neither ring offsets nor visual facing properties caused this failure.

The validator now accepts the earlier casing only at required casing cells. No frame, coil, emitter, interior-air, height, interface-count, or non-edge placement rule was removed. Terminal use explicitly requests bounded server validation, and snapshot diagnostics contain expected/found IDs and world/local positions. Formation logging is explicit-request-only.

Item ports are now one registered block with an output state and stable Forge capability proxies. Earlier port IDs remain available for saved worlds and can also change modes. The native source template is archived without editing the player save. Clean public/reference structures strip all block-entity data.

Four added GameTests load that actual reference, test four rotations, six terminal faces, roof/floor ports, strict missing/obstructed components with coordinates, mode changes through cached proxies, Core reload, and non-loading chunk guards. Execution is performed by the root integration run.

## Files changed for this update

- `compression/CompressionStructure.java`, `CompressionMachineEntity.java` and `CompressionBlock.java`: compatible casing recognition, explicit terminal validation, bounded work and detailed diagnostics.
- `compression/CompressionPart.java`, `CompressionContent.java`, `CompressionPortEntity.java` and `CompressionAssets.java`: one configurable item port, legacy IDs, stable capability proxies and datagen.
- `compression/client/CompressionScreen.java` and `CompressionMenu.java`: separated panels, coordinated slot positions and diagnostic tooltips.
- `item/custom/ConfiguratorItem.java`: localized component styling and explicit port mode switching.
- `block/entity/NovaReactorControllerEntity.java`, `NovaReactorPortEntity.java`, `NovaReactorStructure.java`, reactor menu/screen, packet and block classes: server-side fuel checks, persistent visible port modes and the dedicated reactor shell.
- `reactor/ReactorExpansionContent.java`, `ReactorExpansionAssets.java` and shared data providers: Nova Reactor Chamber, native model links, recipes, loot and translations.
- `tools/blockbench/nova_industrial_designs.py`, `build_nova_industrial.py`, runtime models/textures and editable Blockbench projects: coherent Nova Reactor models and input/output indicators.
- `CompressionDetectionGameTests.java` and `NovaExpansionGameTests.java`: actual player template, valid and invalid layouts, legacy saves, mode switching and start authorization.

## Integrated validation — 2026-11-04

- `runData`: successful; recipes, blockstates, item models, tags and loot regenerated.
- Dedicated Forge server: **309 required GameTests passed**, including all four new actual-NBT regressions and the reactor fuel/shell/mode cases.
- All **30 native Nova industrial models** parsed using Minecraft's BlockModel loader.
- GUI bounds check: 5 buttons and 49 slots fit the screen without intersections; offscreen preview uses the vanilla font atlas.
- MkDocs strict build: successful.

Foreground in-game visual verification remains deferred as previously requested. The offscreen preview verifies layout, not live rendering with arbitrary resource packs.
