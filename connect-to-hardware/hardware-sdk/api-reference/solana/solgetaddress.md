# solGetAddress

## Solana: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.solGetAddress(connectId, deviceId, params)
```

### Params

[Optional common params](../../common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>`  **length is `4`, Here's a special chain.** [more information](../../path-params.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display address of first sol account:

```typescript
HardwareSDK.solGetAddress(connectId, deviceId, {
    path: "m/44'/501'/0'/0'"
});
```

Return a bundle of sol addresses without displaying them on device:

```typescript
HardwareSDK.aptosGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/501'/0'/0'", showOnOneKey: false }, // account 1
        { path: "m/44'/501'/1'/0'", showOnOneKey: false }, // account 2
        { path: "m/44'/501'/2'/0'", showOnOneKey: false }  // account 3
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
        { address: string, path: Array<number> }, // account 2
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
