# Cable waterlogging and extractor filters

All six tiers of energy cables and item transporters support vanilla waterlogging, including bucket filling/draining and placement in water. Existing block IDs, connection properties and block entity data are retained; the new waterlogged property defaults to false.

Resource extractors (Basic through Chaotic) expose an F button that opens an item filter. Green entries are enabled, red entries are disabled. Every available output is listed, with pagination when needed. The Reality Extractor remains unchanged because it collects Reality Cores rather than drawing from a resource loot table.

The server validates menu actions and synchronizes each option through container data. Disabled items are saved as registry IDs in DisabledExtractionOutputs; missing data defaults to allowing all outputs. Allowed outputs retain their relative loot weights. If all items are disabled, production and its FE consumption pause. Disabling the pending output cancels that pending roll and resets progress; items already in the inventory are retained.

Validation covers all cable tiers accepting and retaining water, bucket removal, default filter compatibility, saving/reloading selections, disabled-output exclusion and pausing with all outputs disabled.
