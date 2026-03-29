# Matter.js Fork Stripping Log

## Removed Packages

### @matter/nodejs-ble (removed 2026-03-29)

**What**: Node.js BLE implementation using Noble (@stoprocent/noble) via BlueZ D-Bus binding.

**Why**: Replaced by Quartermaster (poodle64/quartermaster) which implements the abstract `Ble` class directly using vendored Noble HCI internals, bypassing the unreliable BlueZ D-Bus layer.

**Files removed**:
- `packages/nodejs-ble/` (entire directory)
  - NobleBleClient.ts: Noble scanner wrapping BleScannerClient
  - NobleBleChannel.ts: Noble channel wrapping ConnectionlessTransport + BleChannel
  - NodeJsBle.ts: Ble implementation using Noble
  - BleScanner.ts: Re-export of protocol BleScanner for Noble client
  - BlenoBleServer.ts: Peripheral mode (unused by Quartermaster)
  - BlenoPeripheralInterface.ts: Peripheral mode (unused)
  - install.ts: ServiceBundle hook that registered NodeJsBle on Environment

**Dependencies removed**:
- `@stoprocent/noble` (transitive: `@stoprocent/bluetooth-hci-socket`, `@poodle64/dbus-next`)

**Retained (in @matter/protocol)**:
- Abstract `Ble` class, extended by Quartermaster
- `BleScanner`, used by Quartermaster with QuartermasterBleClient
- `BtpCodec`, `BtpSessionHandler`: BTP protocol layer
- BLE constants (UUIDs, MTU, timeouts)

## Patches Retained

- PeerConnection reconnect fix (matter-js/matter.js#3482)
- UUID format matching fix (matter-js/matter.js#3483)
