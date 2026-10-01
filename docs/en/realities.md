# Realities and dimensional stability

[Português](../pt/realities.md)

Each Reality has a tier, terrain profile, resource quality and persistent stability. Its biomes combine vanilla terrain with tier-specific blocks; there are **18 decorative blocks per tier** and **4–10 biomes per Reality** in the current generation.

## Terrain and resource quality

Terrain profiles include **Normal, Islands, Cavernous, Shattered, Abyss and Sky Islands**. Cavernous has a sealed ceiling; Abyss emphasizes deep vertical shafts and increasingly hostile lower layers. Sky Islands contains floating portions of terrain above the void with reduced gravity. Bring building blocks and a way to handle falls.

The scanner reports **DEPLETED, LOW, NORMAL, RICH or ABUNDANT** resource quality. This changes ore and exclusive-deposit generation. DEPLETED Realities can still have creatures and an extractable core.

Generation changes apply to new chunks. Previously explored terrain is not rewritten when the mod changes its generator.

## Stability is a limited resource

Realities start at **100% stability**. There is no passive regeneration, and merely standing inside does not drain it. Actions in the Reality consume stability:

| Action | Stability cost |
|---|---:|
| Extract the Reality's core sample | 50% |
| Remove an exclusive resource deposit | 5% |
| Mine an ore block | 1% |
| Break another ordinary block | 0.5% |
| A mob dies | 0.25% |

Instability effects intensify as stability crosses 75%, 50% and 25%. Fighting indefinitely or clearing large areas can exhaust a region even without a core extraction.

## Collapse

!!! danger "Zero stability starts a 30-second collapse"
    At zero, the Reality starts a **600-tick countdown**. Leave through the return portal before it finishes. Players left inside die and drop their inventory **even with keepInventory enabled**. The collapsed region becomes permanently unavailable.

The countdown persists and can advance while the server is running even if the region is unloaded. Logging out or unloading chunks is not a way to restore it. The exit portal remains usable during the countdown.

**Next:** plan [core extraction](extractor.md) before spending the region's mining budget.
