# btcGetPublicKey

## Bitcoin: get public key

Retrieves BIP32 extended public derived by given BIP32 path. User is presented with a description of the requested key and asked to confirm the export.

```typescript
const result = await HardwareSDK.btcGetPublicKey(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

### Exporting single public key

* `path` — _required_ `string | Array<number>` minimum length is `1`. read more
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `coin` - _optional_ `string` determines network definition specified in coins.json file. Coin `shortcut`, `name` or `label` can be used. If `coin` is not set API will try to get network definition from `path`.
* `scriptType` - _optional_ InputScriptType, address script type

#### Exporting bundle of public keys

* `bundle` - `Array` of Objects with `path`, `coin` fields

### Example

Return public key of fifth bitcoin account:

```typescript
HardwareSDK.btcGetPublicKey(connectId, deviceId, {
    path: "m/49'/0'/4'",
    coin: "btc"
});
```

Return a bundle of public keys for multiple bitcoin accounts:

```typescript
HardwareSDK.btcGetPublicKey(connectId, deviceId, {
    bundle: [
        { path: "m/49'/0'/0'" }, // account 1
        { path: "m/49'/0'/1'" }, // account 2
        { path: "m/49'/0'/2'" }  // account 3
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
        xpub: string,        // xpub in legacy format
        xpubSegwit?: string, // optional for segwit accounts: xpub in segwit format
        chainCode: string,   // BIP32 serialization format
        childNum: number,    // BIP32 serialization format
        publicKey: string,   // BIP32 serialization format
        fingerprint: number, // BIP32 serialization format
        depth: number,       // BIP32 serialization format
    }
}
```

[Read more about BIP32 serialization format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization\_format)

Result with bundle of public keys

```typescript
{
    success: true,
    payload: [
        { path, serializedPath, xpub, xpubSegwit?, chainCode, childNum, publicKey, fingerprint, depth }, // account 1
        { path, serializedPath, xpub, xpubSegwit?, chainCode, childNum, publicKey, fingerprint, depth }, // account 2
        { path, serializedPath, xpub, xpubSegwit?, chainCode, childNum, publicKey, fingerprint, depth }  // account 3
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
