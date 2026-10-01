# Machines, modules and stability

[Português](../pt/machines.md)

Refiners process item recipes; resource extractors provide a separate machine progression. The **Reality Extractor** is another device entirely: it collects dimensional core samples and has its [own guide](extractor.md).

## Three module types

Machines with module support have **three shared upgrade slots**. Each slot takes one module. Identical modules can occupy different slots and their power adds together, up to the normal coverage cap. The items remain non-stackable; you can also mix module types and tiers.

| Family | Effect at full coverage |
|---|---|
| Stability | 75% less stability loss |
| Speed | Up to 4× work rate |
| Energy | 75% less FE/t |

Coverage is the total power of that family divided by the machine's tier demand, capped at 100%. Tier power and demand are **Basic 1, Void 2, Stellar 2, Darklight 4, Nova 8, Zenith 16, Chaotic 32**. For example, a Basic Energy Module on a Darklight machine supplies 25% coverage, reducing its base energy cost by 18.75%.

The slots are shared: filling all three with speed upgrades leaves no slot for energy or stability. Choose a balance your power and maintenance systems can sustain.

## Maintain stability

The Flux Crystal maintenance slot is separate from upgrade slots. Recovery from one crystal becomes smaller at higher tiers:

| Machine tier | Recovery per Flux Crystal | Passive recovery, empty to full |
|---|---:|---:|
| Basic | 25 percentage points | 5 minutes |
| Void | 15 | 10 minutes |
| Stellar | 10 | 20 minutes |
| Darklight | 5 | 35 minutes |
| Nova | 2 | 60 minutes |
| Zenith | 1 | 90 minutes |
| Chaotic | 0.5 | 120 minutes |

Passive timings assume loaded machines and normal 20 TPS. Pausing supported machines does not stop their passive recovery.

!!! danger "Stability at zero"
    If stability reaches zero, the machine explodes.

**Next:** configure a [Stability rule](controller.md) and improve [calibration](controller.md) to reduce operating penalties.
