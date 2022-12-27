# starcoinGetPublicKey

## Starcoin: get public key

Retrieves BIP32 extended public derived by given BIP32 path. User is presented with a description of the requested key and asked to confirm the export.

```typescript
const result = await HardwareSDK.starcoinGetPublicKey(connectId, deviceId, params);
```

## Params

****[**Optional common params**](../common-params.md)****

### Exporting single public key

* `path` — _required_ `string | Array<number>` minimum length is `1`. read more
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

#### Exporting bundle of public keys

* `bundle` - `Array` of Objects with `path`, `showOnOneKey` fields

### Example

Return public key of fifth starcoin account:

```typescript
HardwareSDK.starcoinGetPublicKey(connectId, deviceId, {
    path: "m/101010'/0'/0'",
    showOnOneKey: true
});
```

Return a bundle of public keys for multiple starcoin accounts:

```typescript
HardwareSDK.starcoinGetPublicKey(connectId, deviceId, {
    bundle: [
        { path: "m/101010'/0'/0'" }, // account 1
        { path: "m/101010'/0'/1'" }, // account 2
        { path: "m/101010'/0'/2'" }  // account 3
    ]
});
```

### Result

Result with only one public key

```typescript
{
    success: true,
    payload: {
        path: string, // hardended path
        publicKey: string
    }
}
```

[Read more about BIP32 serialization format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization\_format)

Result with bundle of public keys

```typescript
{
    success: true,
    payload: [
        { path: serializedPath, publicKey }, // account 1
        { path: serializedPath, publicKey }, // account 2
        { path: serializedPath, publicKey }, // account 3
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
