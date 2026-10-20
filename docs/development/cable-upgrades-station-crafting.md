# Cable upgrades and flexible Void Station crafting

## Cable upgrades

Right-click an existing cable with the next cable tier's orb (energy) or circuit (items). One material upgrades up to eight eligible cables in the connected, loaded network, starting nearest the clicked block. Fewer than eight still costs one material. Creative players do not consume materials. Upgrades follow Basic → Void → Darklight → Nova → Zenith → Chaos; there are no Stellar cable tiers. An incorrect material or a maximum-tier cable does nothing.

The bounded breadth-first search respects disconnected sides, waterlogging, build permissions and loaded chunks. It can traverse mixed tiers but only upgrades cables of the clicked tier. It never loads chunks to extend an upgrade. Network reconstruction is suppressed during replacement; capacities are increased in place because topology does not change. Unrelated networks, unloaded cable membership and stored FE remain intact. Registry IDs and existing save formats remain unchanged.

## Crafting

Native Void Station patterns match their occupied bounding rectangle anywhere within the 9×9 grid. Interior empty cells still matter; unrelated extra items invalidate the recipe. Native patterns are translated, not rotated or mirrored.

Ordinary crafting recipes use their original Minecraft/mod CraftingRecipe implementation through a 3×3 view. Shaped and custom crafting patterns may be placed anywhere in the station as long as their occupied rectangle fits 3×3. Vanilla-style shapeless recipes may use up to nine occupied slots anywhere in the full grid. Results are assembled dynamically, preserving recipe-specific NBT; container remainders map back to the actual input slots. Crafting callbacks receive the player when crafting manually.

Ordinary ingredient-based recipes can also be encoded into existing Void Recipe Patterns. Existing IDs and patterns continue working. Special recipes without declarative ingredients remain manual-only: their input cannot safely be inferred for automatic crafting. Recipes from other machine types are not crafting-table recipes and are not imported.

## Optional Polymorph support

The optional integration uses the Forge 1.20.1 Polymorph API, compiled against 0.49.5 (Modrinth Forge version ID V6TZ8mE6). No Polymorph classes are bundled. Its native conflict selector appears beside the manual output, supports the station UI scaling and is hidden in automation mode. Recipe selection is shared by players viewing the same station and is always checked against the current ingredients. Both native station and ordinary crafting conflicts are supported. The automation pattern fixes the selected recipe ID independently of later manual selections.

Development validation: add `-PpolymorphRuntime` to a Gradle run to include Polymorph and enable its development refmap remapping. This version is compatible with the project's existing JEI dependency. Newer Polymorph distributions may require a newer JEI according to their own metadata.

API reference: https://github.com/illusivesoulworks/polymorph/tree/1.20.x/common/src/main/java/com/illusivesoulworks/polymorph/api

Cable collision shapes are cached for the 64 physical connection combinations per block, sharing identical shapes between I/O modes and water states without changing geometry.

## Visible recipe browser and JEI transfers

The station now has a Recipes button in the header. It opens both native Void Station and ordinary crafting categories. The obsolete invisible arrow click area is removed. Without JEI, the button is disabled and explains the missing optional browser.

JEI 3×3 transfers map their first nine logical input slots to the first three columns of the first three station rows. The other 72 manual inputs remain part of the transfer's crafting slots so JEI returns unrelated ingredients to the player instead of leaving them in the grid. Outputs, modules, patterns and automation storage are excluded. Transfers require Manual view; Automation view gives a translated explanation instead of moving items into inactive ghost slots.

Automation input exposes one destination slot per incoming item at a time. This prevents Forge insertItemStacked simulation from counting the same next-craft quota repeatedly across empty slots. The next available destination is recalculated after insertion and after every craft, preserving one-complete-craft replenishment and continuous Auto Repeat production.

Regression coverage: Void Extractor -> Void Transporter -> Void Station completes one Flux craft, waits for missing Stable Particles, then resumes for two more crafts without restarting Auto Repeat. Unrelated raw iron remains in the extractor. This uses controlled extractor inventory and validates sided delivery/resumption, not random extraction timing. All 71 required GameTests passed. The reported permanent stop was not reproduced.
