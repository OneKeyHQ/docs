# xrpGetAddress

## Ripple: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.xrpGetAddress(connectId, deviceId, params)
```

### Params

[Optional common params](../common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>`  minimum length is 5. [more information](../path.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display address of first ripple account:

```typescript
HardwareSDK.xrpGetAddress(connectId, deviceId, {
    path: "m/44'/144'/0'/0/0"
});
```

Return a bundle of ripple addresses without displaying them on device:

```typescript
HardwareSDK.xrpGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/144'/0'/0/0", showOnOneKey: false }, // account 1
        { path: "m/44'/144'/1'/0/0", showOnOneKey: false }, // account 2
        { path: "m/44'/144'/2'/0/0", showOnOneKey: false }  // account 3
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
        publicKey: string,   // account public key
        path: Array<number>  // hardended path
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
