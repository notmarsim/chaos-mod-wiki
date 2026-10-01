# Energy and automation

[Português](../pt/energy.md)

## Values

Base values, before calibration and upgrades:

| Tier | Refiner FE/t | Refiner base cycle | Resource extractor FE/t |
|---|---:|---:|---:|
| Basic | 500 | 300 ticks | 2,000 |
| Void | 4,000 | 150 ticks | 16,000 |
| Stellar | 32,000 | 100 ticks | — |
| Darklight | 256,000 | 60 ticks | 1,000,000 |
| Nova | 2,000,000 | 20 ticks | 8,000,000 |
| Zenith | 16,000,000 | 10 ticks | 64,000,000 |
| Chaotic | 128,000,000 | 5 ticks | 512,000,000 |

| Source | Base output |
|---|---:|
| Basic Coal Generator | 250 FE/t export |
| Void Coal Generator | 2,000 FE/t export |
| Stellar generation | Up to 8,000 FE/t in suitable solar conditions |
| Darklight generation | Up to 64,000 FE/t in darkness |
| Nova Reactor | Up to 4,000,000 FE/t at ideal operating temperature |

For scale, an unmodified Zenith Refiner requires the peak output of four Nova Reactors. A Zenith Extractor requires sixteen. Account for imperfect operating conditions and other loads instead of relying on every source's theoretical maximum.

## Connect the network

Use energy cables to connect generators, storage and consumers. Item transport cables move inventories and serve a different purpose. The current cable families are Basic, Void, Darklight, Nova, Zenith and Chaos.

The Configurator's sneak interaction configures supported block faces. Check face connections when a nearby block does not receive energy. A Machine Controller connected to the energy network can discover compatible machines in loaded chunks; it does not load the world merely to search for them.

Set consumer priority to **HIGH**, **NORMAL** or **LOW** in the Controller. The cable network serves higher priorities first and shares energy within a priority. Direct block-to-block transfers use their own logic.

## Keep an area loaded

The **Dimensional Anchor** is a powered chunk loader, not a Breach's living Reality Anchor. Configure it through the Controller. It supports square areas from **3×3 to 10×10 chunks**, with a cost of `1,000 × side³ FE/t`: 27,000 FE/t for 3×3, up to 1,000,000 FE/t for 10×10. Its buffer holds 256 million FE.

It releases its chunk tickets when unpowered, disabled, removed or located in an unavailable Reality. A chunk loader does not prevent a Reality's collapse.

**Next:** configure [independent automatic rules](controller.md) and [machine maintenance](machines.md).
