# Transporter windows and item animations

Transfer frequency now doubles by tier: Basic 1, Void 2, Darklight 4, Nova 8, Zenith 16 and Chaos 32 pulses/second at 20 TPS. Items per pulse remain 1, 4, 16, 32, 64 and 640, respectively. Maximum throughput per input is therefore 1, 8, 64, 256, 1024 and 20480 items/second, subject to inventory acceptance and available resources.

Each extracting cable has its own fractional pulse clock, so mixed networks retain the input cable's tier. At 32 pulses/second some ticks execute two pulses' worth of transfers. Fractional credit is bounded below one pulse; idle/unloaded inputs never build an overdue burst. Clocks reset on network reconstruction as the previous network timer did.

The existing native transporter meshes retain their rails, collars, dimensions, tier textures and I/O markings. Their opaque interior surfaces are replaced by vanilla cutout glass windows. BuildCableModels preserves these changes on regeneration. No new bitmap assets are required.

Successful inventory transfers generate cosmetic item animations following connected cable paths, including turns and vertical segments. The server commits transfers atomically as before; the animation is a representation of completed transfers, not a second item inventory or a dropped entity. Block/chunk unloading cannot lose or duplicate goods through this visual system.

Limits: 8 visual dispatches per network per 10 ticks, 2048 visited cable positions per path search, up to 126 cable positions per route, only nearby players receive packets, and at most 256 active visual items per client. Long routes or excess traffic may omit animations. Icons use the item type without transmitting item NBT. Animations travel at 4 blocks/second, independent of actual throughput. Client state clears on logout or dimension change.

The network protocol changes from 4.9 to 4.10; update clients and servers together. World save formats and items per pulse are unchanged.
