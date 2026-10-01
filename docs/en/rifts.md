# Reality Rifts and Scars

[Português](../pt/rifts.md)

A Reality Rift is a temporary leak into the **Overworld**. Its colored center marks a tier, nearby terrain gradually changes, and creatures of that tier become much more frequent. This supplements the existing natural tiered-mob spawning system.

## Tiers and rarity

Natural selection uses these weights, **after** the game has decided to attempt a Rift. They are not a per-tick or per-chunk spawn probability.

| Tier | Share of natural tier selection |
|---|---:|
| Unstable | 65% |
| Void | 17% |
| Stellar | 11% |
| Duality | 4.5% |
| Darklight | 1.8% |
| Nova | 0.6% |
| Zenith | 0.1% |

**Stable and Chaos Rifts do not occur naturally.** Natural attempts are also limited by player proximity, active-Rift limits and spacing between centers, so there is no guaranteed discovery interval.

## Sizes and terrain influence

| Size | Natural size selection | Target radius | Typical initial lifetime | Terrain change limit |
|---|---:|---:|---:|---:|
| Minor Rift | 60% | 12 blocks | 3–5 Minecraft days | 256 blocks |
| Reality Rift | 30% | 26 blocks | 6–9 days | 1,400 blocks |
| Major Rift | 10% | 44 blocks | 10–14 days | 4,000 blocks |
| Dimensional Breach | Through escalation | 96 blocks | Must be sealed | 18,000 blocks |

One Minecraft day is 24,000 ticks, normally 20 minutes. A growing Rift starts a new stage. The central effect becomes much larger with size; larger stages also support more creatures.

## Lifecycle and escalation

Most Rifts follow:

```text
ACTIVE → WEAKENING → CLOSING → CLOSED
```

A growth check halfway through a stage has a **10% Minor → Reality**, **8% Reality → Major**, or **5% Major → Breach** chance. These are individual stage checks, not repeatedly rolled every tick. Growth is preceded by a warning and a delay:

> Rift instability increasing...

The final transition warns of major dimensional instability and announces **DIMENSIONAL BREACH FORMED**. Read the [Breach guide](breaches.md) if this happens near your farm.

## Close a Rift

Hold use with a **Stable Orb** within **six blocks** of the center for **five seconds**. Progress pauses if you move away or stop using; it does not reset. One orb is consumed on completion in Survival.

Closing removes the central rupture and stops its additional spawning. It **does not erase existing creatures or restore changed terrain**. The remaining area is a **Reality Scar**. Rare old Scars also generate naturally, with altered terrain and small details such as crystals, dead vegetation or ruins.

## Rewards

Ordinary closures provide a random amount of **Rift Residue** and a small chance of **Rift Core**, increasing with tier and size. Using tier index `t` (Unstable 0, Stable 1, Void 2, Stellar 3, Duality 4, Darklight 5, Nova 6, Zenith 7, Chaos 8) and size index `s` (Minor 0, Reality 1, Major 2):

- Residue range: `1 + s` through `4 + 2 × (t + 1) × (s + 1)`.
- Rift Core chance: `0.5% + 0.3% × t + 0.8% × s`.

An Unstable Minor Rift gives 1–6 residue with a 0.5% core chance; a Zenith Major Rift gives 3–52 with a 4.2% core chance. Rewards appear at the center when its chunk is ticking. Breaches use [larger guaranteed rewards](breaches.md).
