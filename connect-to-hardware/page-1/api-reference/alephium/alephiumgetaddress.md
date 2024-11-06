# alephiumGetAddress

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### Alephium: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is asked to confirm the export on OneKey.

```typescript
const result = await HardwareSDK.alephiumGetAddress(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
    showOnOneKey: false,
    group: 0,
    includePublicKey: true
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>` minimum length is 3
* `group` — _optional_ `number | undefined | null` &#x20;
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`



**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` and  `group` fields



#### Example

```typescript
// Get single address
const response = await HardwareSDK.alephiumGetAddress(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
    showOnOneKey: true
});

// Get multiple addresses
const batchResponse = await HardwareSDK.alephiumGetAddress(connectId, deviceId, {
    bundle: [
        {
            path: "m/44'/1234'/0'/0/0",
            showOnOneKey: false
        },
        {
            path: "m/44'/1234'/1'/0/0",
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
        derivedPath: string,
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
        { address: string, derivedPath: string, publicKey: string, path: Array<number> }, // account 1
        { address: string, derivedPath: string, publicKey: string, path: Array<number> }, // account 2
        { address: string, derivedPath: string, publicKey: string, path: Array<number> }  // account 3
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
