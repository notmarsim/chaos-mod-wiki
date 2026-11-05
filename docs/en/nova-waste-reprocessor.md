# Nova Waste Reprocessor

The Reprocessor converts **10,000 mB Nova Waste into 8 Exotic Dust**. Its default cycle takes **200 ticks (10 seconds)** at **320,000 FE/t**, or **64 MFE per batch**. Values are configurable in `chaosmod-nova-waste-reprocessor.toml`.

## Build the installation

The supplied reference is **3 x 15 x 13**, with two hollow towers connected to a central **3 x 3 x 3** chamber. Rotate the complete installation horizontally, or exchange the two tower functions by exchanging their fluid/item components and cables.

The central Core is at source coordinate **(1, 1, 6)**. Tower bases occupy **X=0..2, Z=0..2** and **X=0..2, Z=10..12**. The central chamber occupies **X=0..2, Y=0..2, Z=5..7**. Keep those horizontal distances.

- Build each tower from hollow **3 x 3 casing rings**, with the center left empty at every ring level.
- Place one isolated **Tower Cap** directly above the empty central column. The eight neighboring cells on the cap level remain air.
- Each tower must be at least **four blocks high including the cap**, with three casing rings below it. Towers can have different heights and extend up to the world's build height.
- Ports can replace the middle cell of a tower side, excluding bottom/top ring edges and all corners.
- Waste Tower ports are Fluid Ports; Dust Tower ports are Item Ports. All ports on a tower share one storage balance.
- At the base's second layer, place an inward-facing tower port toward the central chamber.

The central chamber has exactly one **Reprocessing Core** inside. Its two opposing tower-facing face centers must be the fluid input and item output. The other four face centers accept exactly **one Terminal**, **one to three Energy Ports**, and casing in remaining cells. Every edge and corner remains casing.

Connect the required paths using existing Chaos cables of any tier:

```text
Waste Tower output -> Fluid Cable -> Fluid Cable -> Central fluid input
Central item output -> Item Transporter -> Item Transporter -> Dust Tower input
```

Configure cable faces to **pull from the source** and **push toward the destination**; the cable-to-cable connections must remain open. Their tiers determine actual transport throughput. Physical cable presence alone is insufficient. The Reprocessor does not teleport resources between these inventories.

A clean example is available to operators through `/place template chaosmod:nova_waste_reprocessor`. It contains no saved resources or energy.

## Storage and operation

Each casing-ring layer adds **1,000,000 mB** to the Waste Tower or **65,536 items** to the Dust Tower by default. Capacities are calculated independently. Both balances use persistent 64-bit counters, with ordinary Forge fluid/item interfaces exposing manageable transfer amounts.

Provide FE, supply Waste Tower external Fluid Ports, configure the two cable paths, then click **Start**. The chamber receives waste through its dedicated Fluid Port, pays FE as progress advances, and commits the batch atomically. Completed dust waits in the chamber output until the Item Transporters deliver it to the Dust Tower. External Dust Tower Item Ports allow extraction.

Insufficient FE, missing resources, full destination, unloaded chunks or invalid construction suspend processing safely. **Stop preserves paid progress.** Waste is consumed only when a complete output can be retained. Shrinking a tower never silently deletes excess contents; insertion and production stop until capacity is available. Breaking the Core preserves balances, its FE buffer, pending dust and paid progress in the dropped Core, rather than spilling millions of entities.

The terminal provides Overview, Storage and Diagnostics tabs, including tower capacities/heights, process progress, energy, transfer rates and the location of structural errors. Validation and cache auditing use configurable work budgets and never force-load chunks.

## Exporting reactor waste

Install a **Nova Reactor Waste Fluid Port** on a valid exterior face. In the Reactor terminal select **External Extraction**. The same waste reservoir is then exposed as Nova Waste fluid instead of being converted internally. Connect Fluid Cables to the Waste Tower. Multiple Reactors may supply the same tower. Switching modes never recreates or duplicates waste.

JEI lists the conversion and construction recipes. Forge capabilities allow standard modded pipes, storage exporters and importers to use the external ports.
