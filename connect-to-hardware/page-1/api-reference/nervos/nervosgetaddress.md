# nervosGetAddress



### Nervos: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is asked to confirm the export on OneKey.

```typescript
const result = await HardwareSDK.nervosGetAddress(connectId, deviceId, params);
```

#### Use requirement

* Firmware version required
  * Touch: 4.9.0
  * mini: 3.7.0

#### Parameters

connectId: `string`

* Unique identifier for the connection

deviceId: `string`

* Unique identifier for the hardware device

params: `object`

* path: _required_ `string` - BIP32 path of requested address (e.g. "m/44'/309'/0'/0/0")
* network: _required_`string` - Network name (e.g. "ckb")
* showOnOneKey: _optional_ `boolean` - Whether to display address on device&#x20;

#### Example

```typescript
const response = await HardwareSDK.nervosGetAddress('connect1', 'device1', {
    path: "m/44'/309'/0'/0/0",
    showOnOneKey: true,
    network: "ckb"
});

console.log(response.address);
```

#### Returns

Returns a Promise resolving to:

```typescript
{
    success: true,
    payload: {
        address: string,     // Nervos address
        path: string  // BIP32 path used for derivation
    }
}
```

Error response:

```typescript
{
    success: false,
    payload: {
        error: string,  // Error message
        code: number    // Error code
    }
}
```

