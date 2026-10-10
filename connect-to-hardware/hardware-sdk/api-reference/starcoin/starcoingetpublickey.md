# starcoinGetPublicKey

## Starcoin: get public key

Returns the public key derived from the specified path. Use `showOnOneKey` to request confirmation on the device.

```typescript
const result = await HardwareSDK.starcoinGetPublicKey(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

### Single public key

* `path` — _required_ `string | Array<number>` SDK minimum length is `3`. [Path format](../path.md)
* `showOnOneKey` — _optional_ `boolean` Whether to display the public key on the device. Defaults to `true`.

#### Multiple public keys

* `bundle` - array of objects with `path`, `showOnOneKey` fields

### Example

Return the public key for first Starcoin account:

```typescript
HardwareSDK.starcoinGetPublicKey(connectId, deviceId, {
    path: "m/44'/101010'/0'/0'/0'",
    showOnOneKey: true
});
```

Return a bundle of public keys for multiple starcoin accounts:

```typescript
HardwareSDK.starcoinGetPublicKey(connectId, deviceId, {
    bundle: [
        { path: "m/44'/101010'/0'/0'/0'" }, // account 1
        { path: "m/44'/101010'/1'/0'/0'" }, // account 2
        { path: "m/44'/101010'/2'/0'/0'" }  // account 3
    ]
});
```

### Result

Single-public-key response

```typescript
{
    success: true,
    payload: {
        path: string, // serialized BIP32 path
        public_key: string
    }
}
```

[Read more about BIP32 serialization format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization_format)

Batch public-key response

```typescript
{
    success: true,
    payload: [
        { path: serializedPath, public_key }, // account 1
        { path: serializedPath, public_key }, // account 2
        { path: serializedPath, public_key }, // account 3
    ]
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code?: string | number // error code
    }
}
```
