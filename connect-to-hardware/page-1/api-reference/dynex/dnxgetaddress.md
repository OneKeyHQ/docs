# dnxGetAddress

### Dynex: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is asked to confirm the export on OneKey.

```typescript
const result = await HardwareSDK.dnxGetAddress(connectId, deviceId, params);
```

#### Use requirement

* Firmware version required
  * classic: 3.8.0

#### Parameters

connectId: `string`

* Unique identifier for the connection

deviceId: `string`

* Unique identifier for the hardware device

params: `object`

* path: `string` - BIP32 path (e.g. "m/44'/29538'/0'/0/0") \[required]
* showOnOneKey: `boolean` - Whether to display address on device (default: true)

For batch addressing:

* bundle: Array of objects with above parameters

#### Example

```typescript
// Get single address
const response = await HardwareSDK.dnxGetAddress('connect1', 'device1', {
    path: "m/44'/29538'/0'/0/0",
    showOnOneKey: true
});

// Get multiple addresses
const bundle = await HardwareSDK.dnxGetAddress('connect1', 'device1', {
    bundle: [
        { path: "m/44'/29538'/0'/0/0", showOnOneKey: false },
        { path: "m/44'/29538'/0'/0/1", showOnOneKey: false }
    ]
});
```

#### Returns

Success response for single address:

```typescript
{
    success: true,
    payload: {
        address: string,     // Dynex address
        path: string        // BIP32 path used
    }
}
```

Success response for multiple addresses:

```typescript
{
    success: true,
    payload: [
        { address: string, path: string },
        { address: string, path: string }
    ]
}
```

Error response:

```typescript
{
    success: false,
    payload: {
        error: string,
        code: number
    }
}
```

#### Firmware Requirements

* Minimum firmware version: 3.8.0
