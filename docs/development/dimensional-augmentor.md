# Dimensional Augmentor

The Dimensional Augmentor consumes tier-matched Guardian Cores and FE to grant permanent, individual player upgrades. Its 9×9 Void Station recipe uses Darklight bars, wires, circuits, capacitors and a Machine Casing. It accepts FE and Guardian Cores through Forge capabilities on any side. Capacity: 2 billion FE; input: up to 10 million FE per transfer.

Insert a Core, select one of the three attributes, inspect the current/maximum level, accumulated bonus, next bonus, incremental gain and costs, then press **Augment**. Chaos also offers Limit Expansion. **Stats** browses all tiers without a Core. The menu button packet carries only a selection/action; the server derives and checks every cost, level, tier, distance and resource before committing. Two viewers retain separate selections and player progression.

## Progression and configuration

Values per level are in `serverconfig/chaosmod-augmentations.toml` within the world folder. The Java definitions and safety limits are centralized in `augmentation/AugmentConfig.java` and `AugmentData.java`.

- Base maximum: 10. Chaos expansions unlock 15, 20 and 25, without granting levels.
- Core costs: levels 1–3: 1; 4–6: 2; 7–9: 3; 10: 4. Above 10: `4 + ceil((level - 10) / 3)`.
- FE: `base_fe_cost × 2^tierIndex × desiredLevel²`, starting with Unstable index 0; default base is 10,000 FE. Costs fit the machine buffer.
- Expansion I/II/III: 8/16/32 Chaos Guardian Cores and 250 million / 1 billion / 1.75 billion FE.
- Levels use stable named NBT keys under Forge's persisted player compound, copied on respawn and synchronized on login, respawn, dimension change and upgrade. Existing players without this compound start at zero.

## Effect composition

- HP, armor, knockback resistance and block/entity reach are additive attribute modifiers with deterministic UUIDs. Reapplying never stacks duplicate modifiers.
- Attack speed, movement speed and melee damage multiply the existing attribute total by `1 + bonus`, retaining other mods' modifiers.
- Global amplification multiplies other augmentation bonuses once by `1 + amplification`, up to 25%; it never amplifies itself or triggers effects recursively.
- Mining speed and tool efficiency add together, then multiply the existing break speed.
- Jump velocity multiplies by `sqrt(1 + bonus)` to increase height by the requested percentage.
- Critical chance adds a chance for a melee critical; critical damage adds percentage points to the critical multiplier. Explicit critical denial by another mod is respected.
- Absorption and general resistance add together; projectile/explosion resistance adds only for its damage type. Combined augmentation reduction caps at 80%. Armor and other existing protections still apply independently. Damage that bypasses invulnerability is excluded.
- Armor penetration reduces effective armor by its capped fraction; it does not bypass damage immunities or replace boss health logic.
- Life steal uses final melee damage, bounded by remaining target HP and 10 HP per hit; maximum fraction 25%.
- Area damage affects up to eight visible hostile, non-allied mobs within three blocks of the struck target; maximum 50% and 100 damage per target. Secondary hits cannot cause further area damage, penetration or life steal.
- Health regeneration is HP/second. Hunger regeneration is hunger points per 30 seconds. Pickup preserves vanilla ownership and pickup delay and handles at most 32 nearby item entities per second, within a maximum 32-block range.
- Cooldown reduction caps at 50% and applies only to compatible Chaos Mod abilities (Chaos sword/axe/pickaxe/shovel/hoe, Nova sword and Antimatter). It does not shorten other mods' cooldowns.
- Flight speed modifies existing ability flight speed, capped at 0.5; it grants no flight and does not modify Elytra physics.
- Energy regeneration is a compatible equipment hook: equipped non-block Chaos Mod items exposing a receivable Forge energy capability gain the configured bonus against a 20 FE/second reference (default +1 FE/second per level). Items without energy capability are unaffected; external mods' equipment and carried batteries are excluded.

## Tier bows and shields

All nine tiers have repairable bows/shields (Chaos is indestructible), tier-colored native 3D models and pull/block poses. Darklight and above are fireproof. Lower tiers use crafting-table recipes; Duality and above use the Void Station and consume the previous tier's equipment. Bow damage increases by tier while preserving arrows, charge, vanilla enchantments and ammunition behavior.

Shield durability is rounded up from 1.4× the matching chestplate. Unstable, Stable, Void and Stellar use the Netherite chestplate reference because these tiers have no armor set. No existing registry IDs or player/world data formats were removed.

## Validation

- Forge dedicated GameTest server: 47 required tests passed, including successful/failed transactions, tier validation, exact costs, maximum levels, expansions, independent players, idempotent modifiers, death cloning, full player NBT reload, machine NBT reload, bossbar cleanup, shield durability and blocking, critical compatibility and damage safety limits.
- `runData` regenerated recipes, loot, tags and model links successfully.
- Minecraft's model parser accepted the machine, shield and all four bow poses. The existing Guardian and tier-creature geometry audit also passed.
- English/Portuguese keys and all 18 equipment recipe/model entries were checked.
- The GUI and held-item poses still require visual playtesting in a Minecraft client; automated model validation is not an in-game visual check.
