# Machine Controller and calibration

[Português](../pt/controller.md)

## Which tool should I use?

**Configurator:** right-click a compatible machine to tune **Energy Frequency**, **Field Alignment** and **Phase Offset**. The displayed efficiency tells you whether an adjustment helped. Calibration targets are specific to the machine and saved with it. Efficiency ranges from 85% to 100%; poor calibration can slow processing and increase energy and stability costs.

**Machine Controller:** connect it to your energy cable network and open its panel to manage operating modes, automatic rules, sources, priorities, groups and comparator output. Its front follows placement orientation. The Controller does not calibrate machines and is not a cable bridge.

## Operating modes

| Mode | Behavior |
|---|---|
| ON | Work is enabled without automatic threshold rules; redstone still applies |
| OFF | Work is paused |
| AUTO | Every enabled automatic rule must permit operation; redstone still applies |

Select a machine, choose a metric with **Edit**, enable **Rule: ON**, and set its lower and upper thresholds. Enabling a rule selects AUTO. Numeric edits can be committed with Enter; the adjustment buttons also change values.

### Example: two rules at the same time

Set **Stability 20/90**: stop when stability falls to the lower threshold and resume after recovery to the upper threshold. Then select **Energy 10/95**: stop at the upper energy threshold and resume at the lower threshold. Both rules remain saved and active when you switch the editor; any blocking rule pauses the machine.

Between the two thresholds, the previous decision is preserved. This gap prevents rapid on/off switching. Be careful when monitoring a machine's own energy: if it pauses full and nothing drains its buffer, it may never reach the restart threshold without intervention.

**Source** can monitor another compatible machine or battery in the same network. A missing or disconnected source pauses the affected automation. Groups organize the list; they do not merge machines' buffers.

## Redstone, explained

Redstone is measured **at the selected machine**, not at the Controller.

| Setting | When work is allowed | Example |
|---|---|---|
| IGNORED | Regardless of redstone | Automatic operation without a lever |
| HIGH | Signal strength is greater than zero | Lever on = enabled |
| LOW | No signal is present | Lever on = paused |
| PULSE | For 20 ticks after a low-to-high transition | A button briefly enables work |

PULSE means an approximately one-second permission window at 20 TPS, **not one complete recipe**. A held signal does not repeat it; a new rising edge restarts the window. OFF always blocks work, and AUTO still requires its rules to permit it.

## Reactor and accelerator rules

| Machine | Metric | Intended control |
|---|---|---|
| Nova Reactor | Core Heat / Casing Heat | Stop above the high temperature; resume below the low temperature, in kelvin |
| Nova Reactor | Waste | Stop when waste is high; resume after extraction |
| Nova Reactor | Fuel / Coolant | Stop when reserves are low; resume after replenishment |
| Accelerator | Energy | Reserve protection: pause low, resume high |
| Accelerator | Material | Gate new batches by available input; does not interrupt an already active beam |
| Accelerator | Release | Request collision at the configured charge, from 50% to 100% |
| Accelerator | Collector | Stop when the center rod's energy storage is too full |

Stopping the reactor stops fuel burning, but cooling and generation from residual heat continue. The accelerator can refill its FE buffer while AUTO pauses its work; manual OFF and redstone blocking still block charging. Its Release rule operates on its own beam, not on a remote source.

The Controller only discovers loaded network sections, with a bounded cable search. See [energy and chunk loading](energy.md) if a distant machine disappears from the list.
