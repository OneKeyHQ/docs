# filecoinGetAddress

### Use requirement

* Firmware version required
  * Touch: 3.5.0
  * Classic/Mini: 2.10.0

## Filecoin: get address

Retrieves BIP32 extended public derived by given BIP32 path. User is presented with a description of the requested key and asked to confirm the export.

```typescript
const result = await HardwareSDK.filecoinGetAddress(connectId, deviceId, params);
```

## Params

[**Optional common params**](../../common-params.md)

### Exporting single public key

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../../path-params.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `isTestnet` - _optional_ `boolean` Whether test net.



#### Exporting bundle of public keys

* `bundle` - `Array` of Objects with `path`, `coin` fields

### Example

Return public key of first ethereum account:

```typescript
HardwareSDK.filecoinGetAddress(connectId, deviceId, {
    path: "m/44'/461'/0'/0/0",
    showOnOneKey: true,
    isTestnet: false
});
```

Return a bundle of public keys for multiple ethereum accounts:

```typescript
HardwareSDK.filecoinGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/461'/0'/0/0" }, // account 1
        { path: "m/44'/461'/0'" }, // account 2
        { path: "m/44'/461'/0'/0/1" }  // account 3
    ]
});
```

### Result

Result with only one public key

```typescript
{
    success: true,
    payload: {
        path: Array<number>, // hardended path
        address: string,   // address
    }
}
```

Result with bundle of public keys

```typescript
{
    success: true,
    payload: [
        { path, address }, // account 1
        { path, address }, // account 2
        { path, address }, // account 3
    ]
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
