# Compression Terminal inventory visibility repair

The physical deposit slot is a staging slot: the server moves eligible ingredients into the Core's long-count virtual inventory. The earlier screen rendered only the global processing selection, so a different material could remain stored while disappearing from the visible panel. The snapshot also exposed only selected counts.

The terminal now sends a bounded page with two material entries, each containing its item ID, exact original-unit count, and separate compressed-level I/II counts. Each menu owns its page, and the server resolves selections against the last rows advertised to that viewer. Client actions never define material balances. Page changes and tooltips do not materialize resources.

Manual deposits increment a transient UI notification that reveals that material's page. Automation insertions leave the page being inspected unchanged. Processing selection remains a machine setting and may only change while stopped. Physical deposit/stabilizer/output/player slots retain the same 49-slot layout and ordinary vanilla inventory synchronization; a late client BlockEntity uses placeholder handlers until those slot messages arrive.

The registered terminal ID is unchanged. Its English display name is now **Gravitational Compression Terminal**; its Portuguese name is **Terminal de Compressão Gravitacional**. Formation success remains a server diagnostic and is no longer sent as a player toast. An already valid terminal opens without starting another validation attempt.

Two added GameTests cover manual deposits of multiple materials, paging, per-viewer isolation, stable browsing during automation, exact long counts, both packed levels, selection authorization, reload, and late physical-slot updates. Integrated execution is performed by the root.
