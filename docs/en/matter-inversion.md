# Matter Inversion Chamber

The final chamber is a **closed 13×13×13 Darklight multiblock** based on the current user NBT. It produces Antimatter from independent Stellar and Darklight tanks, with the 3×3×3 inversion core fixed at the geometric center.

## Geometry and orientation

Local coordinates are **X/Z=-6 through +6 and Y=0 through 12**. The central Core is **(0,6,0)**. The reference tanks are centered at X=-4 and X=+4, Z=0 and span **Y=1 through 11**. Their center columns remain air; rendered liquids are not world fluid blocks.

The outer frame's **140 edge/corner positions require Inversion Casing**. The **726 non-edge face positions** accept any mixture of Containment Glass, Inversion Casing and valid terminals/ports. All six faces, including roof and floor, are supported. Each required port and at least one terminal must exist. Internal tank walls, core assembly and emitter positions follow the NBT.

The coordinator determines the geometry's orientation and preserves the physical tank assignment after formation. A terminal's exterior face does not redefine which tank contains which liquid.

## Reference materials and legend

The table and diagrams describe the supplied example, not a mandatory exterior palette or conduit route. The example has **1,093 solid blocks**. Outer face materials, number of ports and routed conduit length can vary.

| Symbol | Reference block | Count |
| --- | --- | ---: |
| `#` | Inversion Casing | 382 |
| `R` | Tank Reinforced Casing | 104 |
| `G` | Containment Glass | 551 |
| `M` | Inversion Matrix Casing | 26 |
| `C` | Matter Inversion Core | 1 |
| `E` | Inversion Emitter | 4 |
| `S` | Emitter Support | 8 |
| `F` | Inversion Fluid Conduit | 12 |
| `T` | Matter Inversion Terminal | 1 |
| `L` | Stellar Fluid Input Port | 1 |
| `D` | Darklight Fluid Input Port | 1 |
| `P` | Energy Input Port | 1 |
| `O` | Antimatter Output Port | 1 |

## Construction and fluid routing

1. Build a closed 13×13×13 shell. Every edge and corner is Inversion Casing; fill non-edge faces with glass, casing or functional components.
2. Reproduce the two 3×11×3 tanks at X=-5 through -3 and X=+3 through +5, Z=-1 through +1, Y=1 through 11. Preserve the reference casing corners and top/bottom perimeter, glass side walls and empty central columns. The two tanks remain separated.
3. Build the exact central 3×3×3 assembly at X/Z=-1 through +1, Y=5 through 7: one Core at (0,6,0), surrounded by 26 Matrix Casings. Keep all four emitters and their eight supports at the illustrated positions.
4. Put at least one Terminal, Stellar Input, Darklight Input, Energy Input and Antimatter Output on any non-edge exterior face positions. Multiple ports share the same storage. Fluid ports still need a valid internal conduit path.
5. Route Stellar and Darklight independently. Each input line must reach its own tank, and each tank must also reach its matching central Matrix attachment. Separate conduit pieces can be linked logically through the same tank, as in the reference. Never let opposite-fluid conduit paths share or touch a face.

Conduit arms connect to other conduits, the matching fluid port, the four middle side-wall positions at each of Y=1,6,11 of a tank, and the two central Matrix attachments from the NBT. The middle tank attachment may remain glass. Arms do not connect to the outer casing/floor, tank corners, unrelated glass, supports, emitters, terminal, energy port or output port. Fluid identities are validated by connectivity, not by the old saved arm properties.

Original input pipe routes are examples and may be moved when ports move. Additional interior conduits may occupy otherwise free cells, but never the tank's central fluid columns or required structural components. Other unused interior cells remain air.

## Reference Y layers

Columns run **X=-6 through +6**; rows run **Z=-6 through +6**, top to bottom. The last row is the reference front. Face glass/casing/controls and conduit routes can be changed within the rules above; rigid interior components cannot.

### Y=0

```text
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
```

### Y=1

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRRR.....RRRG
GR.R.....R.RG
GRRR.....RRRG
G.F.......F.G
G.F.......F.G
G.F.......F.G
G.FF.....FF.G
#TGLGPGOGDGG#
```

### Y=2

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.SES.G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=3

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=4

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=5

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.MMM.RGRG
GG.G.MMM.G.GG
GRGR.MMM.RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=6

```text
#GGGGGGGGGGG#
G...........G
G....SES....G
G...........G
G...........G
GRGR.MMM.RGRG
GG.GFMCMFG.GG
GRGR.MMM.RGRG
G...........G
G...........G
G....SES....G
G...........G
#GGGGGGGGGGG#
```

### Y=7

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.MMM.RGRG
GG.G.MMM.G.GG
GRGR.MMM.RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=8

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=9

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=10

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.SES.G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=11

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRRR.....RRRG
GR.R.....R.RG
GRRR.....RRRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=12

```text
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
```

## Production and automation

Each completed operation uses **1,000 mB Stellar Liquid + 500 mB Darklight Liquid → 1 Antimatter**. Both liquids remain in their tanks during work and are consumed together on completion.

| Mode | Duration | Draw | Total FE |
| --- | ---: | ---: | ---: |
| Safe | 45 s | 300 kFE/t | 270 MFE |
| Balanced | 30 s | 600 kFE/t | 360 MFE |
| Overdriven | 15 s | 1.6 MFE/t | 480 MFE |

Use the existing Stellar Liquefier for both fluids, feeding each matching input port. Both tanks store 128,000 mB independently. The energy buffer stores 2 GFE and the shared input budget is 4 MFE/t. The output has nine slots.

Choose intensity and press **Start** for continuous production. **Stop** suspends consumption, preserves paid progress and allows changing intensity before resuming. Mode changes are blocked while enabled. Missing FE or fluids, a full output or unloaded required chunks safely suspend operation. Spent FE is not refunded.

Fluid ports reject the wrong liquid and all drains. The Antimatter port only extracts output. Chaos cables and other Forge-compatible fluid/item automation can use them. Normal Core drops preserve its tanks, energy, inventory and paid progress. Darklight Liquid sustains this process; no extra stabilizer ingredient is required.

JEI lists all construction recipes and three intensities. Configure values in the world's `serverconfig/chaosmod-matter-inversion.toml`; an active cycle retains its captured costs until completion. The GUI reports structural errors and their coordinates, validation progress and emitter states. The machine does not force-load chunks.

For an operator-only construction reference, use `/place template chaosmod:matter_inversion_chamber`. It uses the imported reference coordinates and has no natural generation. Editable Blockbench MCP projects are in `art/models/matter_inversion/`.
