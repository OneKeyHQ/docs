# alephiumGetAddress

## Use requirement

* Firmware version required
  * Touch: 4.10.0

## Alephium: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.AlephiumGetAddress(connectId, deviceId, params)
```

### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>`  minimum length is 5. [more information](../../../hardware-sdk/api-reference/path.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `includePublicKey`  — _optional_ `boolean` determines if response includes public key. Default is set to `false`
* `group`  — _optional_ `number` .  [more information](https://docs.alephium.org/sdk/block).  Default is set to `0`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` ,`showOnOneKey` , `includePublicKey`, `group`fields

#### Example

Display address of first aptos account:

```typescript
HardwareSDK.AlephiumGetAddress(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0"
});
```

Return a bundle of aptos addresses without displaying them on device:

```typescript
HardwareSDK.AlephiumGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/1234'/0'/0/0", showOnOneKey: false, includePublicKey: false }, // account 1
        { path: "m/44'/1234'/1'/0/0", showOnOneKey: false, includePublicKey: true }, // account 2
        { path: "m/44'/1234'/2'/0/0", showOnOneKey: false, includePublicKey: false }  // account 3
    ]
});
```

#### Result

Result with only one address

```typescript
{
    success: true,
    payload: {
        address: string,     // displayed address
        path: Array<number> // hardended path
    }
}
```

Result with bundle of addresses sorted by FIFO

```typescript
{
    success: true,
    payload: [
        { address: string, path: Array<number> }, // account 1
        { address: string, path: Array<number>, pub: string }, // account 2
        { address: string, path: Array<number> }  // account 3
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
