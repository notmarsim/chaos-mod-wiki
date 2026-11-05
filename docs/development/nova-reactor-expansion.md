# Nova Reactor expansion

The reactor remains an odd cubic multiblock, with a central Core, a casing shell, separated full-height fuel columns, one roof control above each fuel column, and a coolant-filled interior. The 3×3×3 compact reactor remains supported. The historic sizes 3–13 are balance benchmarks; they are not construction limits.

- Discovery measures the exterior face containing the Controller, derives the cubic center, and confirms the central Core. A Controller can occupy any valid non-edge exterior face position, including the roof and floor. The existing Controller block state retains its horizontal facing property for save compatibility.
- Discovery and volume validation share a configurable block-read budget. Validated block positions and states are packed in primitive arrays; chunk availability is checked without loading chunks. Building events invalidate the cache; a rolling state check also catches command/mod changes. An unloaded required chunk suspends work and leaves all resources untouched.
- Full-height modern fuel columns are no longer limited by the old Core unit table. Each controlled column supplies 16 MFE/t of nominal power through size 13. Above that benchmark, each active segment supplies 16/11 MFE/t. Fuel and waste scale with the same active-segment equivalent; configured power scales these quantities together. Actual electrical output additionally considers thermal efficiency, spatial coolant coverage, and waste occupancy. Extra empty volume grants no generation multiplier.
- Buffer capacity retains the exact historic unit table through size 13, then scales with useful interior volume: round(40 × interior volume / 1331) units, at 10 GFE/unit. Fuel, coolant and waste reservoirs scale using the same storage units, with 64-bit balances. Stored resources and FE are retained when a smaller valid shell is built; insertion stops until balances fall below the new capacity.
- Coolant circulation depends on physically connected input Item Ports, local rod/coolant contact, and nearby distribution. The structural coolant model aggregates identical horizontal columns, avoiding a coolant-cells × fuel-columns computation.
- Each energy connection can export through its own cable/receiver limit. The former 10 GFE global extraction ceiling is removed; all ports draw from the same FE balance. The GUI uses split long values and reports total actual export, active fuel segments, useful volume and validation progress.

## Waste extraction

`nova_reactor_waste_fluid_port` is an additional output-only Forge fluid capability. It can replace casing on a non-edge exterior face and shares the Controller's waste balance. One existing numeric waste unit is one mB; old saves are migrated without rescaling.

The Resources page selects **Internal Processing** (default) or **External Extraction**. Internal Processing retains 5000 mB → 1 Exotic Dust through the existing output Item Port. External Extraction disables that conversion and allows Fluid Cables to drain the same reservoir. Simulations do not mutate resources; toggling the mode does not create, clear or restore waste. The Industrial Reprocessor provides a more efficient downstream conversion.

All original registry IDs, BlockEntity IDs and state properties remain unchanged. New resource persistence uses the same NBT keys with long numeric values; reading old integer tags remains supported. Legacy pre-filled layouts retain their compatibility behavior until their actual structure changes.

Meltdown terrain damage scales with reactor size and is processed using a bounded queue without loading chunks. The immediate vanilla shock is kept local; the larger terrain conversion is distributed over ticks.

## Validation

`NovaExpansionGameTests` covers a 17×17×17 reactor with 100 columns, budgeted validation, long nominal generation, multiple exports above the former shared ceiling, exclusive waste processing modes, simulated drains, persistence and integer-save migration. Existing thermal/reactor suites remain relevant balance regressions.
