# Rift Walker combat

The existing entity now uses a server-owned FSM and utility selector, with separate movement, target tracking, short memory, and combo controllers. Entity IDs, spawning, home bindings, health and base attribute balance are preserved.

## Difficulty groups

Unstable, Stable, Void and Stellar share all AI parameters. Duality unlocks parry, dodge and crippling strikes; Darklight/Nova add temporary observation-based adaptation; Zenith and Chaos allow more followups. Settings live in the world's `serverconfig/chaosmod-walker-ai.toml`.

## Attacks

- Rending Claws: alternating timed contact, one hit per victim per swing; advanced chains end in a heavier claw.
- Predator Leap: limited observed movement prediction, locked trajectory, obstacle checks and extra recovery on a miss.
- Rift Step: safe grounded destination, origin/destination particles, twelve-second cooldown.
- Sweeping Rend: frontal 120-degree sweep.
- Feral Rush: telegraphed straight charge, limited travel, swept collision checks.
- Claw Parry: brief frontal 50% reduction, never rear immunity.
- Crippling Strike: 1.5 seconds of level-I slowness.
- Frenzy: one activation per combat below 30% health, default 15% movement bonus. Windups and recovery remain.

Successful specials can lead into short conditional combos. Misses, distance, cooldowns and expiry can break a chain. Advanced memory observes recent visible movement and actual incoming attacks, decays, and is never saved between combats. Nearby Walkers stagger preparation windows.

Client poses use synchronized actions and ages; damage is server-owned. Generic tier teleport, fire, echo and burst attacks are excluded for Walkers so they cannot bypass this moveset or violate base-tier AI parity. Existing numerical resistance and attribute systems remain.

Cooldowns and Frenzy survive reload. Interrupted attacks resume safely in recovery, without replaying hits or stacking the transient speed modifier. No boss bar is added.
