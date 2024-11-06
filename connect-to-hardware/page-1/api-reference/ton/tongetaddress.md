# tonGetAddress

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### TON: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.tonGetAddress(connectId, deviceId, {
    path: "m/44'/607'/0'",
    showOnOneKey: true,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>` minimum length is 3. read more
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `walletVersion` — _optional_ `TonWalletVersion` TON wallet version (default: 3)
* `isBounceable` — _optional_ `boolean` whether address is bounceable (default: false)
* `isTestnetOnly` — _optional_ `boolean` whether to use testnet (default: false)

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

```typescript
// Get single address
const response = await HardwareSDK.tonGetAddress(connectId, deviceId, {
    path: "m/44'/607'/0'",
    showOnOneKey: false,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false,
});

// Get multiple addresses
const batchResponse = await HardwareSDK.tonGetAddress(connectId, deviceId, {
    bundle: [
        {
            path: "m/44'/607'/0'",
            showOnOneKey: false
        },
        {
            path: "m/44'/607'/1'",
            showOnOneKey: false
        }
    ]
});
```

**Result**

Result with only one address

```typescript
{
    success: true,
    payload: {
        address: string,     // displayed address
        publicKey: string,
        path: Array<number> // hardended path
    }
}
```

Result with bundle of addresses sorted by FIFO

```typescript
{
    success: true,
    payload: [
        { address: string, publicKey: string, path: Array<number> }, // account 1
        { address: string, publicKey: string, path: Array<number> }, // account 2
        { address: string, publicKey: string, path: Array<number> }  // account 3
    ]
}
```

Error

```
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
