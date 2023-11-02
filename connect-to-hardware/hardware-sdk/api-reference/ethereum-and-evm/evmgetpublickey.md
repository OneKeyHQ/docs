# evmGetPublicKey

## Ethereum: get public key

Retrieves BIP32 extended public derived by given BIP32 path. User is presented with a description of the requested key and asked to confirm the export.

```typescript
const result = await HardwareSDK.evmGetPublicKey(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

### Exporting single public key

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain. [Reference](https://github.com/ethereum-lists/chains/tree/master/\_data/chains).&#x20;

#### Exporting bundle of public keys

* `bundle` - `Array` of Objects with `path`, `coin` fields

### Example

Return public key of first ethereum account:

```typescript
HardwareSDK.evmGetPublicKey(connectId, deviceId, {
    path: "m/44'/60'/0'/0/0",
    showOnOneKey: true,
    chainId: 1
});
```

Return a bundle of public keys for multiple ethereum accounts:

```typescript
HardwareSDK.evmGetPublicKey(connectId, deviceId, {
    bundle: [
        { path: "m/44'/60'/0'/0/0", chainId: 1 }, // account 1
        { path: "m/44'/60'/0'", chainId: 1 }, // account 2
        { path: "m/44'/60'/0'/0/1", chainId: 1 }  // account 3
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
        publicKey: string,   // BIP32 serialization format
    }
}
```

[Read more about BIP32 serialization format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#Serialization\_format)

Result with bundle of public keys

```typescript
{
    success: true,
    payload: [
        { path, xpub, publicKey }, // account 1
        { path, xpub, publicKey }, // account 2
        { path, xpub, publicKey }, // account 3
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
