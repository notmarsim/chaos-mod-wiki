# Zenith equipment upgrades and ground Rift ritual

Upgrade either the Zenith Bow or Zenith Shield by holding it and using **Shift + right click**, with Zenith Bars in the main inventory. Each item has six independent upgrade levels stored in its item NBT. Old items default to level zero. The upgrades do not use or change the existing sword/armor/tool level system.

| Level | Bars for this step | Total bars | Zenith arrows/s | Shield reflection | Knockback resistance | Recovery after disable |
|---|---:|---:|---:|---:|---:|---:|
| 0 | — | 0 | 1 | 0% | 0% | 5s |
| 1 | 16 | 16 | 2 | 5% | 12.5% | 4.35s |
| 2 | 18 | 34 | 3 | 10% | 25% | 3.65s |
| 3 | 20 | 54 | 4 | 15% | 37.5% | 3s |
| 4 | 22 | 76 | 5 | 20% | 50% | 2.35s |
| 5 | 24 | 100 | 6 | 25% | 62.5% | 1.65s |
| 6 | 28 | 128 | 7 | 30% | 75% | 1s |

Both bows fire immediately on use, then at their server-controlled rate. Zenith starts at 1 arrow/s; Chaos retains 10 arrows/s. The next-shot deadline persists on the item, preventing release/reuse from bypassing the rate. Ammo, Infinity and arrow enchantment behavior remain vanilla-compatible. Chaos base arrow damage increases from 50 to 100, before vanilla Power additions, projectile velocity and critical calculations.

Shield reflection returns the listed fraction of **blocked incoming damage** to the living attacker. It respects the target's armor/invulnerability and does not recursively reflect reflections. Knockback resistance applies while blocking and combines with other existing resistance. Recovery refers to the vanilla shield-disable cooldown, not shield durability or the singularity ability cooldown.

Chaos Shield has 50% reflection, 100% knockback resistance while blocking, and no disable cooldown. Raising it no longer applies the vanilla use-item movement slowdown; sprint-key movement still respects hunger, crouching, collisions and blindness. Its singularity ability is unchanged.

## Ground ritual

Drop **one Black Hole + one Rift Core + one orb** together in the **Overworld**. The black hole must be on the ground and the ingredients within 1.5 blocks. Its existing attraction pulls nearby items inward. A complete Rift recipe takes priority over the Nova Fragment reaction.

Each attempt consumes exactly one of each, including failed rolls:

- 10%: failure, no anomaly.
- 30%: Rift, with conditional size chances of 10% Minor, 30% Reality, 60% Major.
- 60%: Dimensional Breach, starting at 100% stability with normal anchors and lifecycle.

The overall probabilities are therefore 3% Minor, 9% Reality, 18% Major, 60% Breach and 10% failure.

Affinity:

- Basic Orb: 30% Unstable, 30% Stable, 40% Duality.
- Stable Orb is not a ritual ingredient; it retains its existing Rift/Breach sealing function.
- Void / Stellar / Darklight / Nova / Zenith / Chaos Orb: matching tier.
- Nadir is not a ritual ingredient.

The existing maximum of 24 active anomalies and 50-block world-border clearance for anchors are checked before consuming items or rolling. An invalid location or full anomaly limit consumes nothing. Stacked ingredients perform at most one attempt per second per black hole. Manual Chaos creation does not change the unlock condition for natural Chaos Rifts.
