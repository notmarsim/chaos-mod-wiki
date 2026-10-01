# Zenith Accelerator

[Português](../pt/accelerator.md)

The accelerator consumes a large amount of FE to produce **Reality Condensate**, which starts the dimensional-core progression.

## Build a horizontal 7×7 ring

Use **four corners**, **nineteen straight segments**, **one Input** in the middle of a side and **one Center Rod** in the center. Keep the interior clear except for the rod, and keep the structure on one horizontal layer.

```text
C S S S S S C
S . . . . . S
S . . . . . S
S . . R . . I
S . . . . . S
S . . . . . S
C S S S S S C
```

`C` = corner, `S` = straight segment, `I` = Input, `R` = Center Rod, `.` = air. Rotate pieces so their connections form a continuous loop. The Input's branch must point toward the rod; the rod lies three blocks counterclockwise from the Input's facing.

![Zenith Accelerator](../assets/zenith_accelerator.png)

## Supply energy and material

Feed **Zenith** or **Zenith Bars** to the Input and replenish them during operation. The Input holds **512 million FE** and uses **8 million FE per paid work tick**. Its UI has an energy bar with stored energy details.

Beam charge and stored FE are separate measurements. With insufficient FE, progress and material consumption pause. Missing material below 50% beam charge causes charge to decay; at or above 50%, the current charge can be used for collision. Reaching 100% also arms collision. The beam finishes its remaining travel before injection.

Use the [Controller](controller.md)'s **Release** rule for a chosen collision threshold between 50% and 100%. Energy reserve, input material and collector capacity have their own rules.

## Collect the result

### Material efficiency

One Zenith Bar represents **11 Zenith**. The accelerator consumes bars at **1/11 of the Zenith item rate**, while retaining their collision bonuses. An uninterrupted full-charge cycle consumes approximately **44 Zenith or 4 Zenith Bars**: the bar route produces 2.5 times the condensate (rounded down) and recovers 3 times the FE, rewarding the additional ingredients needed to craft bars. Keep at least **45 Zenith or 5 bars** available to avoid triggering early release when the input becomes empty; unused items remain in the Input. Partial charges use the same consumption ratio.

Reality Condensate drops **above the Center Rod**. Automate collection there. The collision's explosion effect does not destroy the structure or the produced item.

| Collision charge | Zenith yield | Zenith Bar yield |
|---|---:|---:|
| 50% | 1 | 2 |
| 60% | 2 | 5 |
| 70% | 4 | 10 |
| 80% | 8 | 20 |
| 90% | 16 | 40 |
| 100% | 32 | 80 |

At full charge, the rod recovers 250 million FE for Zenith or 750 million FE for Zenith Bars, much less than the energy spent on the cycle. Removing or invalidating structure parts aborts the cycle.

**Next:** craft Reality Core vessels and use the [Reality Extractor](extractor.md).
