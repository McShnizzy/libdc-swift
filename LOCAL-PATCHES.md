# Local Patches

This fork carries two local modifications relative to upstream
[deepsealabs/libdc-swift](https://github.com/deepsealabs/libdc-swift),
made by Johannes Disselhoff (McShnizzy) for use in the Manta Dive Log app.
Both are grounded in real measurements against a Shearwater Teric, not
speculative changes. Per-file notices are also present at the modified
locations themselves.

This fork does not automatically track new upstream releases; the
package dependency points at this fork's `main` branch. New upstream
fixes or device support are pulled in manually as needed, re-checking
whether the patches below are still required.

## `Sources/LibDCBridge/src/BLEBridge.m`

Connection timeout increased from upstream's 10 seconds to 30 seconds.
Real-world connection times to a Shearwater Teric were measured
occasionally exceeding 10s.

History:
- `3ae9193` (2026-07-05) — 10s → 20s
- `24f2b4f` (2026-07-05) — 20s → 30s
- `c39f0c9` (2026-07-05) — reverted, to isolate a suspected root cause
  in the consuming app (main-thread blocking), not connection duration
- `9d3ca15` (2026-07-06) — reinstated at 30s after confirming it was
  still needed

## `Sources/LibDCBridge/src/configuredc.c`

`open_ble_device_with_identification()`: skip a redundant second
identification attempt that repeated the exact same configuration
already tried via the stored-config path — this only doubled the
connection-timeout wait without giving the device more time on a
single attempt.

- `1663783` (2026-07-05)
