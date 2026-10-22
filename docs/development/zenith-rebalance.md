# Zenith rebalance and Guardian pursuit

## Accelerator

Only Zenith Bars may be inserted. Existing non-bar inventory contents are preserved and removable; an already moving legacy beam stops accelerating and resolves under the existing charge threshold. Acceleration timing, bar consumption, 8 MFE/t running cost and the 100 TFE collector remain unchanged.

Successful collision energy is `round(10_000_000_000_000 × charge²)`. Condensate is `floor(16 × charge)`. Nonfinite input and charge below 0.5 return zero; charge is capped at 1. The real charge is checked before rounding. Failed beams dissipate, with no refund. Existing collector saturation and item output are retained.

| Charge | FE | Condensate |
|---|---:|---:|
| 49.9% | 0 | 0 |
| 50% | 2.5 TFE | 8 |
| 55% | 3.025 TFE | 8 |
| 75% | 5.625 TFE | 12 |
| 100% | 10 TFE | 16 |

## Machines

| Machine | Base FE/t | Base ticks | FE/operation | Buffer | Input/call and tick budget |
|---|---:|---:|---:|---:|---:|
| Zenith Refiner | 1 GFE | 10 | 10 GFE | 128 GFE | 8 GFE |
| Zenith Extractor | 4 GFE | 40 | 160 GFE | 512 GFE | 32 GFE |

Calibration and module equations are retained. Internal consumption, storage, item capacities, NBT, network transfer and client energy synchronization use long values. Forge int calls and native long calls share the input budget on these two machines; simulations do not spend it. At maximum Speed, before calibration, consumption is approximately 5.196/20.785 GFE/t respectively. Existing native cable networking transfers long amounts and retains its network budget.

## Stabilizer slot

The existing slot now accepts Reality Condensate alongside both Flux Crystals. Slot positions and inventories are unchanged. Automatic consumption uses the existing exact recovery threshold, including recovery at zero before explosion for full-bar materials. One item is consumed, never at full stability.

| Tier | Flux | Empowered | Condensate |
|---|---:|---:|---:|
| Basic | 25% | 100% | 100% |
| Void | 15% | 100% | 100% |
| Stellar | 10% | 80% | 100% |
| Darklight | 5% | 40% | 100% |
| Nova | 2% | 16% | 80% |
| Zenith | 1% | 8% | 40% |
| Chaos | 0.5% | 4% | 20% |

Recovery is additive in percentage points and capped at 100%. The existing automatic threshold prevents wasting materials; e.g. Zenith condensate activates at 60% or below. No new stability subsystem or slot was added.

## Guardian

Eight seconds of continuously observed distance of at least 16 blocks enables a priority Reassembly attempt, even in phase one. It respects cooldown, recovery, line of sight, the arena boundary, loaded terrain and safe destination checks. Returning close or losing sight resets the observation. Reassembly may use the remaining six or more plates, so detached phase-three plates no longer make the move impossible. Long relocations advance within the existing safe teleport distance. The existing scatter/reassembly animation and arrival attack are reused.

## Files changed for this request

All Java paths below are relative to `src/main/java/net/marsim/chaosmod/`:

- energy/ProgressionEnergy.java
- energy/BigEnergyStorage.java
- block/entity/ZenithAcceleratorInputEntity.java
- block/entity/ZenithRefinerEntity.java
- block/entity/ZenithExtractorEntity.java
- block/entity/AbstractExtractorEntity.java
- block/entity/RefinerStability.java
- machine/MachineControl.java
- item/ModItems.java
- item/custom/RealityCondensateItem.java
- screen/ZenithAcceleratorScreen.java
- screen/RefinerStabilityPanel.java
- screen/ExtractorScreen.java
- entity/GuardianCombatMemory.java
- entity/GuardianAttackSelector.java
- entity/GuardianCombatDirector.java
- entity/GuardianMovementController.java
- entity/GuardianAiGameTests.java
- machine/FluxThresholdGameTests.java
- machine/EnergyRebalanceGameTests.java

Resources: `src/main/resources/assets/chaosmod/lang/en_us.json`, `pt_br.json`.
Documentation: this file.

Client animation, UI and third-party mod interoperability still require a real game session; automated tests do not certify visual quality.

Validation: all 141 required Forge GameTests passed; Gradle build succeeded. Automated coverage includes the collision curve and threshold, invalid numbers, capped output, bar-only insertion, segmented input budgets, condensate tier recovery/persistence, full-Speed machine input and sustained-distance eligibility/cooldown. No manual client or third-party compatibility session was run.
