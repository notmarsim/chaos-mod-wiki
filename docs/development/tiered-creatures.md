# Tiered creatures and production upgrade

Vanilla mobs keep their original entity IDs, models, AI, equipment, taming and species data. Villagers, wandering traders and Endermen are excluded. Existing tiered Endermen are restored to vanilla attributes while preserving their health fraction and player-assigned names. Players and armor stands are not mobs. Existing registered Chaos Mod zombie, skeleton, creeper and spider IDs remain valid.

Every eligible non-boss mob in a Reality receives that region's affinity. Newly spawned vanilla mobs elsewhere use the existing tier chance profile. Ordinary mobs loaded from older saves are not randomly rerolled; already tiered mobs migrate once, preserving their current health fraction. New tiered Withers and Ender Dragons are command-only; vanilla boss summoning remains unchanged. Existing saved bosses are retained.

`TierMobRules.PROFILES` supplies speed, penetration and fallback stats for species outside the requested hit table. `TierCombatBalance` overrides health/damage for the listed combat species, Walker and Anchor; the following health/damage multipliers are therefore fallback values only. Multipliers use the vanilla species' default attributes, not previously scaled saved values. Health above Minecraft's 1024 attribute ceiling is represented by proportionally reducing incoming damage. Armor penetration bypasses only the listed portion of armor; toughness, enchantments and resistance still apply. The damage cap is applied before mitigation.

| Tier | Health multiplier / cap | Damage multiplier / cap | Speed multiplier / cap | Armor penetration |
| --- | --- | --- | --- | --- |
| Unstable | 7 / 500 | 6 / 24 | 1.12 / 0.55 | 5% |
| Stable | 8 / 600 | 7 / 28 | 1.15 / 0.58 | 7% |
| Void | 10 / 800 | 8 / 36 | 1.20 / 0.62 | 10% |
| Stellar | 12 / 1000 | 9 / 42 | 1.25 / 0.66 | 13% |
| Duality | 18 / 2500 | 12 / 60 | 1.30 / 0.70 | 17% |
| Darklight | 32 / 5000 | 18 / 90 | 1.35 / 0.75 | 22% |
| Nova | 48 / 8000 | 28 / 140 | 1.42 / 0.80 | 27% |
| Zenith | 125 / 20000 | 40 / 220 | 1.50 / 0.85 | 33% |
| Chaos | 650 / 100000 | 70 / 400 | 1.65 / 0.95 | 40% |

Damage and health use HP, not hearts. Flying speed has a separate 1.2 cap. Walkers use the stronger species reference described below, with a 0.33 base movement speed. Wisps remain non-aggressive, have zero attack damage and never trigger retaliatory pulses.

## Abilities

- **Unstable:** 12% chance to schedule a follow-up hit 12 ticks later; 4-second cooldown, melee reach and line of sight required. Follow-up hits cannot recursively trigger abilities.
- **Stable:** 80% base knockback resistance.
- **Void:** short combat teleport attempt every 6 seconds, staying inside loaded chunks, world bounds and the mob's home restriction.
- **Stellar:** successful attacks ignite their victim for 3 seconds.
- **Duality:** alternates every 5 seconds between 30% damage reduction and 20% extra attack damage, within the tier cap. Combat particles indicate the phase.
- **Darklight:** successful attacks reduce armor by 20% for 5 seconds. Refreshes duration without stacking or damaging equipment.
- **Nova:** taking 18% of maximum health within 5 seconds schedules a 4-block energy pulse. 12-second cooldown; no terrain destruction. Targets the aggressor/current target and nearby players for hostile mobs, while respecting allies and line of sight.
- **Zenith:** heals up to 1% maximum health every 2 seconds, with a persistent lifetime budget of 25%; 35% chance to reject a newly applied harmful effect.
- **Chaos:** combines the above combat abilities, with 90% knockback resistance and 50% harmful-effect resistance.

## Additional non-hostile drops

Vanilla drops remain intact. Progression drops use the existing system only for hostile creatures (the vanilla `Enemy` classification, plus hostile Rift creatures).

`TierCreatureLoot` generates nine empty shared loot tables. Add pools there and run `runData`, or override the tables in a datapack:

```
data/chaosmod/loot_tables/entities/tiered/non_hostile/stellar.json
```

An optional species-specific table is also evaluated:

```
data/chaosmod/loot_tables/entities/tiered/non_hostile/stellar/cow.json
```

Both tables use the entity loot context, including victim, location, damage source and killer. For example, a cow-only table can add food without affecting other creatures:

```json
{
  "type": "minecraft:entity",
  "pools": [{
    "rolls": 1,
    "entries": [{ "type": "minecraft:item", "name": "minecraft:cooked_beef" }]
  }]
}
```

To ship a species table with the mod, add it to `TierCreatureLoot.generate`, rather than editing generated output directly.

## Machine update

- Item and energy side modes are independent. The side editor shows adjacent blocks; right/left follow the placing player's perspective. Existing saved world-side configurations remain valid. Machines without an I/O button can be configured with Shift + right-click using the Configurator.
- Side wrappers preserve the mod's long FE transfer API and update cached pipe connections when modes change.
- The **Void Automation Module** only enables automation. Put a valid recipe in the manual 9×9 grid, insert a **Blank Void Pattern**, and choose **Encode Pattern**. Eight separate **Void Recipe Pattern** slots store recipe IDs, with one selected at a time. The Automation tab displays ghost ingredients and an independent 81-slot input buffer in three pages. Forge item capabilities expose inputs 0–80 and completed output 81; unpaid manual previews cannot be extracted.
- **Selected** performs one craft per activation. **Auto Repeat** continues while ingredients and output space are available. **Auto Stock** stops before exceeding the configured 1–64 items in the station’s output (it does not count external inventories). Maximum throughput is two crafts per second. A complete recipe and its container remainders are planned before any consumption. Pipes are limited to one recipe’s worth per matching item to avoid first-material flooding.
- Removing the module leaves patterns intact. Removing a selected pattern pauses production. Old saved-recipe machines migrate the recipe into a pattern and move their automated ingredients into the new buffer once.
- The module recipe is `BCB / ROR / BCB`: B = Void Bar, C = Void Circuit, R = Redstone, O = Basic Orb. Create mechanical-crafting copies are no longer generated.
- Calibration now has five independent axes and a ±5 displayed-unit tolerance per axis. New axes on existing machines start matched, preserving existing perfect calibration.
- Natural meteor chance is reduced by 80%. The Orbital Deflector has 27 salvage slots, charging one deflection cost only when the entire meteor's drops fit. Full storage falls back to the existing deflection behavior.

Client and server must both use this update because machine configuration packets changed. Registry IDs and old inventory layouts are retained; new saved fields are optional for old worlds. Third-party transport integration uses Forge capabilities, but each external modpack still needs its own gameplay validation.


## Species combat reference

`TierCombatBalance` contains the requested species endurance and lethal-hit targets. Unstable, Stable, Void and Stellar use a Stable sword and full Netherite. Duality and above use matching sword/armor, with the Zenith sword and every armor piece at level 21. Reference attacks are fully charged, noncritical and unenchanted, on Normal difficulty with 20 player health. Intrinsic armor-set Strength is included (Darklight +3, Nova +12); Chaos uses a Chaos axe (150 base damage plus 12 from the armor set’s Strength IV), not the instant-kill sword. Real fights vary with weapon cooldown, enchantments, shields, projectile distance, equipment, poison, fire, alternating defense and finite regeneration.

Health uses an effective-health budget above Minecraft's 1024 health-attribute limit. Damage is solved against the reference armor, toughness, custom armor reduction and partial penetration. Rift Walkers target 39 incoming hits and 2.7 outgoing hits, compared with Zombie 35/3, and retain their faster movement. Anchors use a 45-hit budget and scale to the same equipment reference.

Full Chaos armor preserves flight and immunity to ordinary sources; Chaos-tier creatures can now target and damage its wearer. The Chaos sword instantly kills on left-click and with its Shift + right-click area ability, including Chaos-tier mobs. Existing creative/spectator and full Chaos armor protections remain.

The creative-only command is `/tiermob spawn minecraft:zombie void [x y z]`. It accepts eligible vanilla species and registered Rift creatures; command-spawned creatures retain the requested tier even inside a different Reality. Example: `/tiermob spawn chaosmod:rift_walker nova ~ ~ ~`. Endermen and villagers are rejected.

Additional native 3D cuboid anatomy is attached to vanilla renderers: crests and temple plates, shells, spines, wing shards, mandibles, ribs and shoulder crystals. Native animation and species identity remain; palettes follow affinity. Existing four-family models retain their specialized geometry. These are code-defined models, not external Blockbench files.

## Module art

`src/main/resources/assets/chaosmod/textures/item/void_automation_module.png` was generated using the built-in image generation tool, with transparent background. Prompt: “Create a single Minecraft mod inventory item sprite: Void Automation Module. Crisp chunky pixel art, square industrial circuit module, charcoal/graphite Void Bar frame, silver Void Circuit traces, tiny cyan Basic Orb, two red redstone conductors. No text, perspective or background.” The user approved local conversion to a 64×64 RGBA texture with nearest-neighbor resampling; the original generated image remains preserved. Item models and the blank-pattern recipe are generated by the Java datagen providers.


The blank and encoded patterns now have separate transparent sprites, generated with the built-in image tool. Exact prompts and project paths are recorded in `void-pattern-art-prompts.json`. The recipe symbol is illustrative; the actual result appears in the pattern tooltip. Both item models come from datagen.

Hostile geometry coverage also includes Zombie Villagers, Giants, all three Wither heads and an Ender Dragon ornament rig that follows vanilla neck, jaw, wing and leg animation. The geometry audit bakes 612 anatomy components, 9 Dragon rigs and the 36 existing complete four-family variants without missing bones. This validates structure, not in-game visual positioning.

All three generated item textures (Module, blank Pattern and encoded Pattern) are packaged at 64×64 with transparency, as approved by the user.

Zenith upgrades from level 0 to 21 consume 64 bars per upgradeable item: 2 bars for each of levels 1–5, 3 for 6–15, and 4 for 16–21. Further upgrades remain available, increasing cost every six levels. Existing levels are preserved. Zenith reference damage is 95 including Strength I; armor and toughness respect vanilla caps (30/20), with Resistance I. Chaos reference damage is 162 including Strength IV.

## Thermal protection and module capacity

Items registered with Darklight/Nova/Zenith/Chaos/Chaotic tier prefixes, including block items and machine modules, use vanilla fire-resistant item properties. Each correctly equipped armor piece from the four upper armor tiers gives 25% fire-tagged damage reduction; a mixed full set cancels fire-tagged attacks. Generic tier armor reductions skip fire damage to avoid double-counting this rule. Other vanilla mitigation still applies.

Machine module demand is a shared budget of three native-tier modules: 3/6/6/12/24/48/96. Each kind's coverage is its installed power divided by the larger of budget and total installed power. This preserves old overweight inventories while scaling their combined benefits to at most 100% coverage. New insertion rejects overweight configurations; each slot still holds one module.

Basic stability loss is one internal unit per normal working tick without modules (40,000 maximum); three matching stability modules reduce it to 0.25. Fractional loss and whether the prior tick worked are saved. Recovery skips a tick after work, preventing the previous recovery/consumption equilibrium during sustained operation.
