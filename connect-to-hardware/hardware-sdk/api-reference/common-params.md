# Common Parameters

Most chain methods accept a connection identifier, a device identifier, and an object containing both method-specific options and common options:

```typescript
const response = await HardwareSDK.btcGetAddress(connectId, deviceId, {
  path: "m/84'/0'/0'/0/0",
  coin: 'btc',
  showOnOneKey: true,
  useEmptyPassphrase: true,
});
```

* `connectId` is the transport routing identifier returned by [searchDevices](basic-api/search-devices.md). Rediscover it when reconnecting; do not treat it as a permanent physical-device identity.
* `deviceId` identifies the initialized device. In the legacy [getFeatures](basic-api/get-features.md) response, read `payload.device_id`. Discovery may return it as `deviceId`; Bluetooth discovery can return `null` until initialization. Resetting or wiping a device may change this identifier.

Device-management and basic methods have their own signatures. Check each method's reference before passing these arguments.

## Common options

| Option               | Type      | Purpose                                                                                               |
| -------------------- | --------- | ----------------------------------------------------------------------------------------------------- |
| `retryCount`         | `number`  | Maximum number of connection-polling retries.                                                         |
| `pollIntervalTime`   | `number`  | Connection-polling interval in milliseconds.                                                          |
| `timeout`            | `number`  | Timeout for an individual polling attempt.                                                            |
| `keepSession`        | `boolean` | Keeps the transport session after the call. This does not select a wallet.                            |
| `passphraseState`    | `string`  | Opaque state identifying the intended passphrase wallet. See [Passphrase](../advanced/passphrase.md). |
| `useEmptyPassphrase` | `boolean` | Set to `true` to select the standard wallet, which uses an empty passphrase.                          |
| `initSession`        | `boolean` | Requests session initialization. Do not use it as a substitute for selecting the intended wallet.     |
| `deriveCardano`      | `boolean` | Requests Cardano seed derivation for the session; Cardano methods set this as needed.                 |

Do not log PINs, passphrases, wallet session identifiers, or raw signing payloads. Handle `success: false` before accessing a method's success payload.
