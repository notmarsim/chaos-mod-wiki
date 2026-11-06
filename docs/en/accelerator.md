# Zenith Accelerator

[Português](../pt/accelerator.md)

The current Zenith Accelerator is a **49 × 5 × 49** multiblock. It uses Zenith Bars to generate FE and Reality Condensate. Supply external FE for ignition, then maintain fuel and output space. Energy values and phase durations are configurable in `chaosmod-zenith-accelerator.toml`.

## Construction

Select **Zenith Accelerator** in the [Multiblock Viewer](multiblock-viewer.md) for the full layered layout and materials. It uses its own Casing, Foundation, Acceleration Coils, Containment Coils/Pylons, Beam Guides and Reaction Core. Keep the marked beam channels clear.

Terminal, Fuel Injector and Energy Output Ports occupy the four interface columns at X/Z **(7,7), (41,7), (7,41), (41,41)**, using **Y1–Y3** relative to the structure origin. The old terminal/injector positions are casing. Install exactly **one Terminal**, at least **one Fuel Injector** and at least **four Energy Output Ports**. Additional ports share the existing aggregate output limit.

## Operation

Supply FE and Zenith Bars through a Fuel Injector, or use the terminal inventory. Compressed Zenith Bars are accepted through the existing compressed-matter system. The Injector also exposes the Reality Condensate output for item transport.

Click **Start**. The machine progresses through Ignition, Acceleration and Convergence before Generating. Select its power and monitor energy, fuel, output and structural diagnostics. **Stop** initiates shutdown/cooldown. Blocked output or insufficient resources prevent further production.

The ring, beams and central reaction object are visual effects. Their positions follow the real beam channels and the machine's rotation.

## Retired components

The former **Accelerator Input, Center Accelerator Rod, straight segment and curve** are removed from the creative tab, JEI ingredient list and production recipes. Their registry entries remain readable to preserve existing worlds and inventories.

| Retired component | One-way crafting conversion |
| --- | --- |
| Center Accelerator Rod | Zenith Reaction Core |
| Accelerator Input | Zenith Accelerator Terminal |
| Straight segment | Zenith Acceleration Coil |
| Curve | Zenith Accelerator Foundation |

Old saved blocks remain compatible; new installations should use the current multiblock.
