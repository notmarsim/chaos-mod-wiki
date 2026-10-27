# Logistics and recursive Void Station update

## Player-facing changes

- Added Item Splitter and Item Merger, with native three-dimensional block models and dedicated configuration screens.
- Added Coordinate Teleportation Device, a Zenith-tier return device with a transparent 32×32 item texture.
- Gateway favorites, custom names and manual ordering are stored per player. Search matches name, original ID or tier; a favorites-only filter is available. Arrow controls reorder within the favorite/non-favorite groups, keeping favorites first. The underlying discovered list and reality IDs are unchanged.
- Added operating sounds for all refiners, all extractors including Reality Extractor, Basic through Darklight generators, and the active Nova Reactor. Critical reactor temperature replaces its normal loop with the alarm. Sounds follow the Blocks volume category.
- Void Station now has one 81-slot material inventory, displayed over three pages, and a separate completed-output slot. Both manual and automatic crafting use this same material inventory. The 9×9 grid contains configuration ghosts, never paid ingredients.
- Expanded the pattern library from the previous eight slots to exactly nine. The Automation Module enables automation; patterns store recipe IDs and output previews, not resources.
- Added bounded recursive crafting with dependency-first execution, progress, next-result preview, cancellation and persistent queues.

## Void Station controls

1. Put materials into shared storage, manually or through pipes.
2. Use JEI's transfer button to configure a recipe and transfer available ingredients from your inventory. Alternatively, click a ghost slot while holding an item; right-click clears that ghost without consuming the held item.
3. Click **Craft** to put a completed manual result in the output. No module or pattern is required. Crafting-table and native 9×9 recipes remain distinct and are validated by their actual recipe implementation. Polymorph remains the conflict selector.
4. For automation, install the module and encode blank patterns. Insert an encoded pattern in the encoding slot to overwrite that item in place. Use the nine patterns as the library, including intermediate recipes. Select the final recipe, switch to Automation, choose a mode and Start.
5. **Selected** requests the displayed quantity, rounded up to complete recipe batches. **Auto Repeat** continually plans one final recipe batch. **Auto Stock** targets the output-slot quantity. +/- changes the requested amount; Shift changes it by 64. The allowed request range is 1–4096, subject to the plan limit.
6. Cancel keeps completed products and intermediate materials. Reserved slots and pattern removal are locked during a queued plan; unreserved materials remain available and incoming items are accepted. Output can always be extracted. If output or intermediate storage becomes full, the current operation pauses without partial consumption.

## Planning and persistence

`StationCraftPlan` plans against a private inventory copy. A plan is limited to 16 dependency levels, 256 operations and a finite search budget. Matching handles repeated/alternative ingredients and preserves shared materials where possible. Candidate dependencies are checked for cycles; the finished plan is replayed against the original snapshot before acceptance to verify intermediate storage and returned containers. The real inventory is not touched by planning.

Each queued operation consumes a complete validated ingredient set and commits remainders plus its result atomically. Intermediates and returned containers use a private nine-stack crafting buffer, never shared storage; only terminal products enter output. Requests run in on-demand batches. Logical per-slot reservations protect upcoming ingredients; manual crafting uses only unreserved stock. Existing timing is preserved: one automatic operation every 10 server ticks while machine controls permit work. The original Void Station has no FE crafting charge; this update does not introduce one. Cancellation releases reservations without rolling back completed recipes. Queues persist recipe IDs, concrete ingredient grids, progress, reservations, private buffer contents and terminal/intermediate flags. Cancellation returns buffer items to input, then an empty output, retaining unplaceable items in bounded recovery and blocking new automation until recovered.

The Forge item capability exposes 81 input slots and the output slot. External extraction is allowed only from output. This prevents other mods from removing reserved materials. No Create Mechanical Crafter integration was added for exclusive 9×9 recipes. JEI transfers use a dedicated server-validated recipe-ID packet instead of transferring real items into ghost slots.

Migration uses `StationInventoryVersion = 2`. Old automation storage is retained, real items from the old manual grid are merged into it, and any excess is saved in a recovery list. That list drains into available storage and drops its remaining items when the block is broken. Old preview outputs are never migrated as products. Ghosts do not drop and cannot be extracted.

Special manual recipes without an ordinary ingredient list, such as tool repair, use the concrete ghost item data and consume matching real items. Such recipes cannot be encoded for recursive automation; they do not expose a dependable ingredient contract.

## Routing architecture

Routers are explicit network boundaries. Ordinary item networks cannot bypass the router buffer; the router discovers loaded cable paths on each configured face and services the endpoints itself. Discovery is bounded to 512 cable nodes and 64 endpoints per branch. Cached paths and endpoint block entities are checked before transfer. Six persisted input buffers, per-input source cursors, output cursor and weight credit prevent losing items during congestion or reload.

Splitter modes: Round Robin, Priority, Balanced and Weighted Round Robin. Merger modes: Round Robin and Priority, with one output face. Priority/weights are configured per face (1–16). Balanced falls back to round-robin if an endpoint cannot expose reliable capacity. Transfer budgets follow the adjacent cable's pump rate and pulse frequency. Direct inventories use 4 items per pulse and 2 pulses/s.

Use the Configurator to set cable directions: a cable supplying the router must output toward it; a cable receiving from the router must input from it. Physically connecting an independent bypass around a router naturally creates a separate route outside its rules.

## Teleportation balance and safety

- Default capacity and teleport cost: **100,000,000,000 FE (100 GFE)**.
- Recharge acceptance: **128,000,000 FE per Forge receive call**; the source controls actual delivery frequency.
- Server config: `chaosmod-logistics.toml`, configurable capacity from 1 MFE to 10 TFE.
- Sneak + right-click saves exact position, dimension and view direction, including at zero charge.
- Right-click requires a full charge. Energy is drained only after successful server teleportation; saved coordinates remain.
- Destinations must have safe support, free collision space, no hazardous/fluid landing area and valid world boundaries.
- Cross-dimension travel is deliberately disallowed because the mod has no reliable per-player dimension-access authorization API. Within Rift Realities the device also stays within its current reality region. Collapsed realities are rejected.

Recipes are in datagen: Splitter = Basic Circuit surrounded by eight Void Bars; Merger = Basic Orb surrounded by eight Void Bars. Device = twelve Zenith Bars, two Zenith Capacitors and one Zenith Circuit in the supplied 9×9 layout.

## Asset provenance

The two router models are native block-model JSON using existing textures. Device artwork was generated with the built-in image generation tool, then alpha-cropped and converted to transparent 32×32 PNG with the user's explicit approval. Final texture: `src/main/resources/assets/chaosmod/textures/item/coordinate_teleportation_device.png`.

Art prompt: “Create a Minecraft mod inventory item sprite: Coordinate Teleportation Device, a compact handheld technological return beacon, front view slightly diagonal, pixel art with crisp large pixel clusters suitable for 32x32 downsampling. Zenith tier palette: polished silver and white metal casing, light gray shadows, a tiny pale cyan locator screen with a magenta crosshair, two small cyan-magenta capacitor accents, dark graphite outline. Recognizable device silhouette with antenna and small central circuit, premium Minecraft vanilla-adjacent hand pixel art, not photorealistic, no text, no glow outside silhouette, no background, square canvas, generous transparent margin. Only one item, no grid or variants.”

The five supplied MP3 files were converted to mono 44.1 kHz Vorbis OGG. Loops stop on inactivity, unload, dimension change and distance, with a heartbeat timeout and no duplicate loop per machine.

## Validation and limits

- Datagen succeeded (`build/logistics-datagen.log`). All recipes/models/loot generation live in datagen; generated files can be rebuilt.
- The regression suite covers routing fairness/conservation, blocked and disconnected paths, device recharge and persistence, private Gateway preferences, manual/special recipes, recursive dependencies, alternative/shared ingredients, cycles, full storage, container remainders, migration overflow and queue reload/resume.
- Final validation: **155 required GameTests passed; BUILD SUCCESSFUL**. Log: `build/logistics-release-validation.log`. Output: `build/libs/chaosmod-1.2.3.jar`. `git diff --check` passed.
- Client screens, visual models and sound playback have not been manually exercised inside a running Minecraft client. AE2/Create/Mekanism integration follows the sided Forge item capability; full modpack interaction was not exercised in these server tests.
- Teleport tests use a FakePlayer override to verify transaction logic; they are not a real remote-client connection test.
- Dependency planning is intentionally bounded. A chain exceeding the limits is refused without consuming materials; requests must be split into smaller batches.
- Network protocol is now **4.13**; clients and server must use the same build.

## Changed files

- `src/main/java/net/marsim/chaosmod/ChaosMod.java`
- `src/main/java/net/marsim/chaosmod/ClientModEvents.java`
- `src/main/java/net/marsim/chaosmod/block/ModBlocks.java`
- `src/main/java/net/marsim/chaosmod/block/entity/ModBlockEntities.java`
- `src/main/java/net/marsim/chaosmod/block/entity/NovaReactorControllerEntity.java`
- `src/main/java/net/marsim/chaosmod/block/entity/StationCraftPlan.java`
- `src/main/java/net/marsim/chaosmod/block/entity/VoidAutomation.java`
- `src/main/java/net/marsim/chaosmod/block/entity/VoidStationEntity.java`
- `src/main/java/net/marsim/chaosmod/client/MachineLoops.java`
- `src/main/java/net/marsim/chaosmod/compat/JEIChaosModPlugin.java`
- `src/main/java/net/marsim/chaosmod/compat/StationGhostTransfer.java`
- `src/main/java/net/marsim/chaosmod/compat/StationPolymorph.java`
- `src/main/java/net/marsim/chaosmod/compat/StationPolymorphClient.java`
- `src/main/java/net/marsim/chaosmod/compat/StationRecipeTransferInfo.java`
- `src/main/java/net/marsim/chaosmod/datagen/ModBlockStateProvider.java`
- `src/main/java/net/marsim/chaosmod/datagen/ModBlockTagGenerator.java`
- `src/main/java/net/marsim/chaosmod/datagen/ModItemModelProvider.java`
- `src/main/java/net/marsim/chaosmod/datagen/ModRecipeProvider.java`
- `src/main/java/net/marsim/chaosmod/datagen/loot/ModBlockLootTables.java`
- `src/main/java/net/marsim/chaosmod/item/ModCreativeModTabs.java`
- `src/main/java/net/marsim/chaosmod/item/ModItems.java`
- `src/main/java/net/marsim/chaosmod/logistics/CoordinateTeleportationDevice.java`
- `src/main/java/net/marsim/chaosmod/logistics/GatewayFavorites.java`
- `src/main/java/net/marsim/chaosmod/logistics/LogisticsConfig.java`
- `src/main/java/net/marsim/chaosmod/logistics/LogisticsGameTests.java`
- `src/main/java/net/marsim/chaosmod/logistics/RoutingBlock.java`
- `src/main/java/net/marsim/chaosmod/logistics/RoutingEntity.java`
- `src/main/java/net/marsim/chaosmod/logistics/RoutingMenu.java`
- `src/main/java/net/marsim/chaosmod/logistics/RoutingRoutes.java`
- `src/main/java/net/marsim/chaosmod/logistics/RoutingScreen.java`
- `src/main/java/net/marsim/chaosmod/machine/MachineControl.java`
- `src/main/java/net/marsim/chaosmod/machine/ProductionUpgradeGameTests.java`
- `src/main/java/net/marsim/chaosmod/machine/StationCableUpgradeGameTests.java`
- `src/main/java/net/marsim/chaosmod/machine/StationRecursiveGameTests.java`
- `src/main/java/net/marsim/chaosmod/machine/StationTransferFluxGameTests.java`
- `src/main/java/net/marsim/chaosmod/machine/VoidStationRepeatGameTests.java`
- `src/main/java/net/marsim/chaosmod/network/ModMessages.java`
- `src/main/java/net/marsim/chaosmod/network/packet/DeleteGatewayRealityC2SPacket.java`
- `src/main/java/net/marsim/chaosmod/network/packet/GatewayPreferencesS2C.java`
- `src/main/java/net/marsim/chaosmod/network/packet/GatewayRenameC2S.java`
- `src/main/java/net/marsim/chaosmod/network/packet/SelectGatewayRealityC2SPacket.java`
- `src/main/java/net/marsim/chaosmod/network/packet/StationRecipeC2S.java`
- `src/main/java/net/marsim/chaosmod/screen/GatewayMenu.java`
- `src/main/java/net/marsim/chaosmod/screen/GatewayScreen.java`
- `src/main/java/net/marsim/chaosmod/screen/ModMenuTypes.java`
- `src/main/java/net/marsim/chaosmod/screen/VoidStationMenu.java`
- `src/main/java/net/marsim/chaosmod/screen/VoidStationScreen.java`
- `src/main/java/net/marsim/chaosmod/sound/MachineAudio.java`
- `src/main/java/net/marsim/chaosmod/sound/MachineSoundPacket.java`
- `src/main/java/net/marsim/chaosmod/sound/MachineSounds.java`
- `src/main/java/net/marsim/chaosmod/transport/ItemNetwork.java`
- `src/main/resources/assets/chaosmod/lang/en_us.json`
- `src/main/resources/assets/chaosmod/lang/pt_br.json`
- `src/main/resources/assets/chaosmod/models/block/item_merger.json`
- `src/main/resources/assets/chaosmod/models/block/item_splitter.json`
- `src/main/resources/assets/chaosmod/sounds.json`
- `src/main/resources/assets/chaosmod/sounds/machines/extractor.ogg`
- `src/main/resources/assets/chaosmod/sounds/machines/generator.ogg`
- `src/main/resources/assets/chaosmod/sounds/machines/reactor.ogg`
- `src/main/resources/assets/chaosmod/sounds/machines/reactor_alarm.ogg`
- `src/main/resources/assets/chaosmod/sounds/machines/refiner.ogg`
- `src/main/resources/assets/chaosmod/textures/item/coordinate_teleportation_device.png`


## Router connection and UI follow-up

- Replaced the text-only six-face controls with a relative-face diagram showing each adjacent block's item icon. Selecting a face shows its world direction, neighbor name, input/output mode and priority/weight. Right-clicking a face cycles its mode. The screen scales down for smaller windows.
- Router cores are now 8×8×8 pixels (cable cores are 4×4×4), with independent arms extending only toward connected inventories/cables. Shapes and multipart models use the same connections. Existing routing settings and buffers remain in their original world directions. New routers orient their front when placed.
- Removed the stretched machine-facade texture mapping in favor of existing Void cable shell/panel textures. Multipart blockstates remain generated by datagen.
- New cable connections pick the router's corresponding flow direction. Changing router faces aligns adjacent non-disconnected cables. Explicit cable disconnects remain respected.
- Configurator targeting now treats hits beyond the cable's actual 6–10 pixel core as hits on the arm, including its top surface near the junction. An EAST arm hit no longer falls back to UP. Explicit NONE settings survive neighbor changes; cable updates read the live world state to preserve other properties such as waterlogging.
- Shared-network destinations are assigned their shortest output branch instead of the first enumerated face. Balanced uses round-robin tie-breaking for equally occupied destinations. Output credit is bounded and retained across pulse phases rather than discarded each tick.
- Added regression coverage for branch selection, equal-load consumers, cable-fed merger inputs, arm extension/retraction and Configurator face isolation.


### Gateway and cross-dimensional teleport follow-up
- Gateway renaming is now inline through the row action, confirmed with Enter or the checkmark; Escape cancels. The permanent lower rename panel was removed.
- Favorited realities cannot be deleted by their viewer, with validation in the server packet handler as well as the UI.
- The coordinate device resolves the saved destination dimension and permits different Reality regions. Missing dimensions, unavailable realities, unsafe terrain and insufficient charge still reject the operation. Energy is consumed only after arrival in the destination level.
- Router transfer animation budgets now cover each branch independently, so a busy input cannot hide the other output's transfers.
- Regression run: 162 GameTests, 161 passed. All router regression tests passed; `capturePullDamageAndCleanup` remains failing (Guardian capture movement assertion). No Guardian gameplay changes were made in this follow-up. Visual behavior still needs in-game review.


### Solar output, portable tiers and router enclosure
- Stellar Generator distributes stored energy and charges its item independently of solar generation conditions. Darkness, roofs and weather still prevent new FE production.
- Teleportation Device retains its registry ID, 100 GFE default capacity and teleport behavior; receive limit increases from 128 MFE/t to 1.28 GFE/t.
- Splitter/Merger retain the compact core and conditional arms, with reinforced corners, recessed routing diagrams and distinct lime/cyan indicators. Models use native JSON geometry and existing Minecraft/mod textures.
- Tier identity is restored before spawn selection from persistent NBT, vanilla entity tags or serialized name metadata (for bucket/capture implementations that retain names). Legacy formatted names support migration. Capture tools that discard all identifying entity data cannot preserve an identity through this mechanism.
- Forge conversion events carry the original tier and custom names into the new species, refresh generated names and recompute species-specific stats. Cached stats now include the entity type.

Validation: final build successful; all 167 GameTests passed, including legacy conversion repair. Client-side visual review was not performed.


## Reload and routing corrections

Router capabilities are recreated on revival. Saved face settings and cursors survive reload; cable directions are synchronized with router faces, respecting explicit disconnections. Transient missing machine capabilities and unloaded neighboring chunks no longer erase saved cable modes. High-tier routing uses cable pulse frequency and pump rate, splitting large pulses into stack packets. Visual transfer speed follows the cable tier per path segment.


## Manual crafting controls

Drag an item across the 9x9 grid to paint recipe cells without consuming the cursor stack. Drag with an empty cursor to clear cells, or use Clear for the whole grid. Manual crafting draws exclusively from the unreserved shared inventory. The quantity field requests up to 4096 output items, stopping at complete recipe batches, available ingredients or player inventory space. Shift-click the manual result or Craft to produce at most one normal item stack (or fewer when resources are insufficient). Bulk results go directly to the player's inventory. Quantities use a dedicated server-validated VarInt packet, because vanilla menu button IDs are signed bytes.
