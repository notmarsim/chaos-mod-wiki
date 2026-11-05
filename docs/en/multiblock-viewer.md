# Multiblock Viewer

The **Multiblock Viewer** plans structures and projects a construction guide into the world. Craft it at the **Void** tier, supply FE, and right-click it to open the library. It never places blocks or spends construction materials.

## Power

| Property | Default |
| --- | ---: |
| Buffer | 256,000 FE |
| Maximum input | 2,048 FE/t |
| Standby | 4 FE/t |
| 3D preview in use | 8 FE/t |
| Hologram projection | 32 FE/t |

Only the highest-priority operating cost applies. A projection costs **32 FE/t total**, including when a preview is also open. The physical Viewer pays for the projection; the building site needs no power connection.

Without power, the basic screen remains accessible, but the 3D preview, hologram and construction HUD stop. Saved projects retain their position, orientation and settings. Breaking or unloading the source Viewer stops its projection. No chunks are force-loaded.

## Library and preview

Search by machine name or filter by tier. Select a machine to inspect its actual block models, material counts, required interfaces and structural alternatives. **Back** preserves the library filter and scroll position.

A **+** after a tier identifies an upgradable family. These families also appear under their supported higher-tier filters; selecting one applies the filtered tier to its initial plan.

Drag to orbit the preview, scroll to zoom, and use **Center** to reset the camera. The category filters can hide casings, glass, ports and other components; individual block types can also be hidden. Transparency helps expose internal components. Hover a block to inspect its name, registry ID, relative coordinate and permitted replacements.

The layer controls show all layers, one selected layer, or all layers up to the selection. Large previews use a bounded sample of the structure; select individual layers for closer inspection. Render and validation budgets limit work, rather than imposing a new machine-size limit.

The library covers:

| Family | Configuration |
| --- | --- |
| Stellar Liquefier | Fixed structure |
| Stellar Reactor | Fixed structure |
| Industrial Refinery | Rectangular dimensions and processing tier |
| Industrial Extractor | Rectangular dimensions and processing tier |
| Stellar Energy Matrix | Rectangular dimensions and component tier |
| Event Horizon Forge | Fixed structure and permitted assembly interfaces |
| Matter Inversion Chamber | Fixed geometry and flexible shell interfaces |
| Darklight Plasma Generator | Fixed geometry and permitted interface positions |
| Nova Reactor | Size, fuel-column count, spacing and offsets |
| Nova Waste Reprocessor | Independent waste/dust tower heights |
| Gravitational Compression | Fixed structure and flexible exterior interfaces |
| Zenith Accelerator | Track, curves, Input and central Rod |
| Reality Gateway | Frame and Terminal |

The Viewer does not grant access to recipes or bypass the actual machine's tier, construction or operating rules.

## Free Build and Planned Build

**Free Build** is the default. Purple positions are flexible: any replacement accepted by that machine is valid. The suggested casing, glass or port distribution is a guide. Required interface totals are tracked separately, so filling every flexible position with casing does not satisfy a missing Energy Port.

Use **Planned Build** when you want a specific arrangement. Inspect a flexible position and cycle its permitted alternatives before starting. The saved projection then checks that choice as part of the plan. Fixed frames, corners and internal components retain their machine-specific rules.

## Build in the world

1. Select **Build**. The screen closes and a draft hologram follows the block you point at.
2. Rotate the draft and confirm its position. Cancel returns control without placing anything.
3. Supply and place the real blocks yourself. The guide tracks correct, missing, incorrect and unloaded positions.
4. Use layers and the remaining-material HUD to complete the structure. Automatic layer advancement can move to the next completed layer.
5. **Structure Complete** appears only after the real structural validator also accepts the arrangement. Formation and operation still belong to the machine itself.

Holograms use green for correct blocks, blue for missing blocks, red for incorrect blocks, and purple to identify flexible placement. Unloaded positions are unknown; they are never counted as empty or complete. The hologram contains no physical blocks or entities.

Default controls are rebindable in Minecraft's controls menu:

| Action | Default |
| --- | --- |
| Confirm / cancel draft | Alt + Enter / Alt + Backspace |
| Rotate draft | Alt + R |
| Cycle layer mode | Alt + L |
| Next / previous layer | Alt + Up / Alt + Down |
| Cycle missing/incorrect filter | Alt + V |
| Toggle correct blocks | Alt + C |
| Minimize / hide HUD | Alt + M / Alt + H |
| Automatic layer advancement | Alt + J |
| Pause / stop project | Alt + P / Alt + Delete |
| Reopen Viewer | Alt + B |
| Scroll remaining-material list | Alt + mouse wheel |

## Saved projects and servers

Each player has their own saved projects, with one active at a time. **Projects** allows resuming or deleting them. Closing the screen, disconnecting or restarting the server does not erase a project. A stopped project can be resumed from a powered Viewer.

The server owns project validity and FE consumption. Placement controls describe a guide; they cannot create blocks, consume other players' materials or alter machine inventories. Projects can refer to another loaded dimension, but neither their source nor construction chunks are kept loaded automatically.

Energy, work budgets, cache sizes, rendering distance and project limits are configured in `chaosmod-multiblock-viewer.toml`. A cache-budget warning asks the operator to increase the technical budget for an enormous project; it does not change the multiblock's legal construction dimensions.
