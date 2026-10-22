# Dimensional Guardian combat AI

The existing Guardian entity, collapse binding, attributes, core drops and tier palettes are retained. Combat is server-authoritative and safe for old saves.

## Architecture

- `GuardianCombatDirector`: hierarchical state machine (`IDLE`, `TARGET_EVALUATION`, `APPROACH`, `ATTACK`, `DEFEND`, `REPOSITION`, `RECOVERY`, `RETURN_TO_BREACH`). Attacks contain windup/execution/recovery stages.
- `GuardianAttackSelector`: utility scoring, individual cooldowns and temporary repetition penalties. Illegal attacks are removed before scoring; bounded noise only breaks near-ties.
- `GuardianPlateController`: twelve independent plate resources and their leases, reservations, repair timers and states.
- `GuardianMovementController`: throttled pathfinding, lateral destinations, pursuit bounds and return to the original breach position.
- `GuardianCombatMemory`: decaying observations of distance, melee/projectile pressure, attack frequency, block frequency and lateral movement. Hidden players are investigated at their last visible location.
- `GuardianComboController`: conditional followups with expiry, resource/cooldown checks and mandatory recovery windows.
- `GuardianFormation`: existing attack timelines, effects and synchronized formations. Physical strikes reuse `RiftMeleeGoal` without its pursuit behavior.

Utility evaluations occur at a configurable reaction interval rather than every tick. Paths are requested no more than once per second. No attack logic runs on the client.

## Difficulty groups

| Group | Tiers | Default decision interval | Default recovery | Maximum combo followups |
|---|---|---:|---:|---:|
| Base | Unstable, Stable, Void, Stellar | 20 ticks | 44 ticks | 0 |
| Intermediate | Duality | 16 ticks | 36 ticks | 1 |
| Advanced | Darklight, Nova | 12 ticks | 30 ticks | 2 |
| Elite | Zenith | 10 ticks | 26 ticks | 2 |
| Ultimate | Chaos | 8 ticks | 24 ticks | 3 |

The first four tiers share **exactly the same configuration, phase unlocks, reaction times, defensive patterns and strategy**. Existing numerical tier attributes and colors remain different; Stellar no longer adds a unique damaging trail to the base group. Complexity increases only from Duality onward.

Phase one (above 70%) uses melee, Barrage, Slam and Fortress. Phase two (35â€“70%) adds Cyclone, Reassembly, Rain and Spear to every group; Duality+ also gains Collapse; Darklight+ gains Guillotine and Orbit Detonation. Phase three (at/below 35%) permanently removes four plates from core defense and lets them attack independently when not reserved. Phase progression cannot be undone by healing or reloading.

## Plate resource and defense

Each plate has one state: `ORBITING`, `ATTACKING`, `RETURNING`, `SHIELDING`, `DISABLED` or `REASSEMBLING`. States are packed and synchronized with the formation and combat stage. At most twelve plate projectiles can hold valid leases.

Each defending plate reduces incoming damage by 2.5%, before normal armor: 12 = 30%, 6 = 15%, 2 = 5%. Fortress doubles the contribution and blocks frontal hits during its active interval. Accumulated frontal damage breaks plates; players can also attack plates in flight or disable armed mines. Disabled plates recover after eight seconds by default. Committed offensive plates cannot also protect the core.

## Attacks and counterplay

- **Orbital Barrage:** six curved sequential flights; defense falls while they are away.
- **Fracture Slam:** six plates gather on one arm; an expanding ground wave deals damage and launches grounded victims approximately **10 blocks upward** at normal gravity. Jumping above the wave or leaving its radius avoids it. Darklight+ produces a second delayed wave. Ceilings and modified gravity affect actual height; normal fall-damage rules remain intact.
- **Shard Cyclone:** an expanding plate ring damages nearby players and finishes with four outward launches.
- **Plate Fortress:** slowly tracking frontal defense; flanking bypasses its directional block. Surviving plates launch when it ends.
- **Gravitational Collapse:** ten plates form a sphere, pull visible players and then release a telegraphed blast; escape beyond the blast radius.
- **Dimensional Reassembly:** panels scatter, briefly become invulnerable and relocate only to a loaded, grounded, collision-free destination inside the arena. A delayed wave follows. Its individual cooldown is much longer than normal attacks.
- **Plate Guillotine:** six existing plates form two opposing three-plate walls that converge on a marked location.
- **Dimensional Spear:** three plates converge on a marked position in a fast thrust.
- **Orbit Detonation:** six existing plates travel to marked positions, arm for 35 ticks, explode without editing blocks, then return. Destroying a plate prevents its detonation.
- **Fragment Rain:** ten plates rise above marked regions and dive toward the ground.

Combo examples: Barrage â†’ approach â†’ Slam; Collapse â†’ Rain; Fortress â†’ Guillotine. Higher groups can follow Rain with Spear, and elite groups can follow Spear with Slam, subject to their configured chain length. Every link still needs available plates, range, line of sight, individual cooldown and a recovery interval. Offensive animations are not freely cancelled.

## Configuration and persistence

Server settings are generated at `<world>/serverconfig/chaosmod-guardian-ai.toml`:

- `pursuit_radius` (80 blocks by default).
- `unseen_target_memory_ticks` (160).
- `disabled_plate_recovery_ticks` (160).
- `fracture_slam_height` (10).
- Per-group reaction/recovery, combo length, adaptation weight, ideal distance and cooldown scale. Base-group adaptation and combo length are fixed to zero.

The chase leash does not heal the boss or reset its phase. Neither navigation nor teleportation force-loads chunks. Reload preserves phase, disabled-plate timers and remaining individual cooldowns, but resets transient combat/memory to a safe idle state. Old projectile leases become invalid, preventing duplication.

Particles, active plate entities, delayed blasts and residual zones are bounded. Existing registry IDs are unchanged. Automated tests cover group parity, utility/cooldown rules, plate ownership/states, combo bounds, observation expiry, adaptation, leash behavior, actual Slam launch/jump avoidance, mine telegraphs, reloads and existing combat behavior. Server tests do not replace in-game visual review.


## Advanced attack corrections

Eligible attacks accumulate a bounded priority bonus while skipped, preventing perpetual starvation by higher-scoring patterns. Failed reassembly setup has a short retry cooldown. Reassembly selects a grounded, loaded, collision-free destination before reserving plates. Guillotine plates travel to opposite sides and hold briefly before crossing the marked position. Spear plates form an aligned assembly aimed at the marked target.

Guardian base damage defaults to 3.5 times the same-tier Walker reference, configurable as `damage_relative_to_walker` in `chaosmod-guardian-ai.toml`. Effective damage remains correct above vanilla's attack-attribute ceiling; special attacks apply their own multipliers to this value. Existing tier/phase eligibility remains in effect.


## Guillotine capture

When a player remains between the closing pair, those two physical plate leases hold the player and slowly pull them toward the Guardian (0.07 blocks/tick). Damage ticks once per second at 8% of the Guardian's base attack; arrival deals 200%, with a minimum five-second opportunity to escape. Rapidly press the bound Sneak key 15–20 times; holding it counts only once. Progress decays after 1.5 seconds without a new press, and the action bar shows the count.

Destroyed plates, dead/removed Guardian, player death, logout, dimension changes and a 20-second timeout release the capture. Captures are transient and never saved into a permanent player effect.

## Command summoning

Use vanilla /summon with named Tier NBT, for example:
- /summon chaosmod:dimensional_guardian ~ ~ ~ {Tier:"nova"}
- /summon chaosmod:reality_wisp ~ ~ ~ {Tier:"void"}
- /summon chaosmod:rift_walker ~ ~ ~ {Tier:"chaos"}
- /summon chaosmod:reality_parasite ~ ~ ~ {Tier:"duality"}

Allowed names: unstable, stable, void, stellar, duality, darklight, nova, zenith, chaos. Legacy numeric Affinity/GuardianTier saves still load. Summoned Guardians establish their home at their summon position instead of requiring a Breach. Natural spawning remains collapse-only.

Command availability is refreshed after respawn and game-mode changes. This does not grant permissions: /summon still needs vanilla command permission, and /realityrift remains creative-only.
