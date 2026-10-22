# Zenith Accelerator dashboard

- Overview: input FE buffer, beam charge, and normalized beam speed, with segmented bars and exact FE tooltip.
- Statistics: actual draw, active cost, buffered energy runtime, full-charge FE and condensate, current-charge yield, collector fill, and material type.
- Start / Stop is validated by the server against the open menu and player distance.
- New installations wait for Start. Existing saves without OperationEnabled retain their automatic operation. The setting persists through reload.
- At 50% charge or higher, Stop keeps the beam moving at its current speed until the next Input crossing, then routes it to the center. Below 50%, Stop, loss of material or loss of power makes the beam decelerate and disappear with Failed status, without FE or condensate. It stops further material consumption and acceleration. Rewards scale with charge only for valid collisions at 50% or higher. During pre-launch preparation, Stop cancels without a collision. The stop request persists, and prevents a new cycle until Start is pressed. Removing the ring still invalidates the cycle safely.
- Machine Controller/redstone gates remain authoritative; Start does not override them. Input energy can still refill while stopped. Controller shutdown also routes an active beam. Loss of input power dissipates weak beams or releases sufficiently charged beams instead of freezing them.
- No I/O shortcut is present in the Accelerator dashboard. Shift + Configurator remains available.
- Layout: 300 x 238, two tabs, dedicated material/control column, and inventory below the diagnostics.
- Rewards shown at full charge are potential rewards, not a guarantee of collector free space. Runtime assumes continuous active consumption with no incoming FE.

Validation: AcceleratorDashboardGameTests covers a real ring, Start, Stop, Input crossing, no further material consumption while stopping, one collision, persistence, legacy defaults and rejected remote/unknown actions. Logs: build/accelerator-dashboard-final.log. Interactive in-game visual QA is not included.
