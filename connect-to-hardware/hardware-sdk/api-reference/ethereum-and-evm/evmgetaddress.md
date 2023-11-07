# evmGetAddress

## Ethereum: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.evmGetAddress(connectId, deviceId, params)
```

### Params

[**Optional common params**](../../common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>`  minimum length is 5. [more information](../../path-params.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain. [Reference](https://github.com/ethereum-lists/chains/tree/master/\_data/chains).&#x20;

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display address of first ethereum account:

```typescript
HardwareSDK.evmGetAddress({
    path: "m/44'/60'/0'/0/0",
    chainId: 1
});
```

Return a bundle of ethereum addresses without displaying them on device:

```typescript
HardwareSDK.evmGetAddress({
    bundle: [
        { path: "m/44'/60'/0'/0/0", chainId: 1, showOnOneKey: false }, // account 1
        { path: "m/44'/60'/1'/0/0", chainId: 1, showOnOneKey: false }, // account 2
        { path: "m/44'/60'/2'/0/0", chainId: 1, showOnOneKey: false }  // account 3
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
