# Stellar Liquefier and Stellar Reactor

The Stellar fuel chain is **Stellar Bar → Stellar Liquid → artificial sun → FE**. All construction materials are Stellar or earlier. The existing Stellar Generator remains the compact, fuel-free daytime option.

## Default production

| Machine / injection | Fuel or input | Time | Energy |
|---|---|---|---|
| Liquefier | 1 Stellar Bar → 1,000 mB | 1,200 ticks / 60 s | 16,000 FE/t consumed |
| Reactor Low | 1,000 mB | 1,200 ticks / 60 s | 128,000 FE/t generated |
| Reactor Normal | 1,000 mB | 300 ticks / 15 s | 256,000 FE/t generated |
| Reactor High | 1,000 mB | 150 ticks / 7.5 s | 512,000 FE/t generated |

These are steady-state values at 20 TPS. One, four or eight Liquefiers sustain Low, Normal or High respectively. Ignition ramps the sun and generation up gradually. Fuel is prepaid from the tank using persistent fractional accounting; an empty tank cannot create unpaid energy. A full output buffer pauses further fuel use.

One Liquefier consumes 19.2 MFE per bar. The steady-state reactor produces 153.6 MFE per bucket on Low, or 76.8 MFE on Normal/High, before liquefaction and ignition costs. The chain therefore provides horizontal scaling with an efficiency tradeoff.

## Build the 3×3×3 Liquefier

Use 21 Stellar Liquefier Casings, 1 Controller, 1 Liquefaction Core, and one of each Liquefier port: Item Input, Fluid Output, Energy Input and Stabilizer.

1. Make a closed 3×3×3 shell. All edges and corners are casing.
2. Put the Liquefaction Core in the central block.
3. Place the Controller at the center of a vertical face, facing outward.
4. Place the four ports at four other face centers. The remaining face center is casing.
5. Supply Stellar Bars, FE and Flux Crystals. Open the Controller and press Start.

The tank stores 32,000 mB and the energy buffer stores 64 MFE by default. Only one bar is processed at a time. Missing energy, a full output tank or unloaded required chunks preserve the paid job and its progress. Extract fluid from the Fluid Output Port using compatible pipes or an empty bucket.

## Build the 11×11×11 Reactor

The structure has a closed shell, one central Reactor Core, six Field Emitters and twelve Containment Coils. All other interior positions must be air. The Controller must be at the center of a vertical face, facing outward.

The following reference uses coordinates **0–10**, measured from the lower north-west corner; X points east, Y up and Z south.

| Part | Position |
|---|---|
| Controller, facing north | (5, 5, 0) |
| Fluid Input Port | (0, 5, 5) |
| Energy Output Port | (10, 5, 5) |
| Stabilizer Port | (5, 5, 10) |
| Reactor Core | (5, 5, 5) |

Use Reactor Casing on every edge and corner. Other shell positions may be Reactor Casing or Stellar Reinforced Glass, except the Controller and ports. A fully glazed example uses **116 casings, 482 glass blocks, 1 Controller and 3 ports**.

Put Field Emitters at (1,5,5), (9,5,5), (5,1,5), (5,9,5), (5,5,1) and (5,5,9).

Put the twelve Containment Coils at:

- XY plane: (1,1,5), (1,9,5), (9,1,5), (9,9,5).
- XZ plane: (1,5,1), (1,5,9), (9,5,1), (9,5,9).
- YZ plane: (5,1,1), (5,1,9), (5,9,1), (5,9,9).

Other faces/orientations work by rotating this layout. The sun stays centered, growing from about 2 to 5 blocks across according to injection. It is rendered directly; no world blocks are created to represent it.

## Ignite and automate

Fill the Reactor through its Fluid Input Port. Supply ignition FE **to the Controller**, and insert Flux Crystals through the Stabilizer Port or GUI. Press Start with at least **4,000 mB**, **5 MFE** and a compatible stabilizer available. The 4,000 mB is a minimum stored quantity, not an extra fuel charge.

The GUI controls Low/Normal/High injection and shows actual FE/t, fluid consumption, storage, containment and Stability. The Energy Output Port supports existing Chaos cables and Forge FE systems. Fluid ports implement Forge IFluidHandler for compatible tanks and pipes. Chaos Fluid Cables can connect these ports; configure Pull beside the Liquefier output and Push beside the Reactor input.

Both machines reuse Stellar Stability and its automatic crystal thresholds. Higher reactor injection increases Stability loss. At zero Stability, the normal machine-failure behavior applies. Jobs, tanks, energy and fractional fuel credit persist across saves. Unloaded required chunks suspend the machine without force loading them.

Use the **?** tooltip in either GUI for a construction reminder. JEI lists construction recipes, liquefaction and the three reactor fuel modes. Reactor Core/Controller recipes use the Void Station; the other construction recipes use a crafting table.

## Configuration

Defaults are configurable in `serverconfig/chaosmod-stellar-energy.toml` for each world/server. It includes liquefaction time/yield/energy, tank and FE buffers, ignition, formation time, export limits and each mode's fuel period, generation and Stability work. Keep tank/buffer capacities large enough for configured recipes and ignition requirements.
