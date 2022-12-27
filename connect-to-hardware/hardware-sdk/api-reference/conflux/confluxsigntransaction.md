# confluxSignTransaction

### Conflux: Sign transaction <a href="#cardano-sign-transaction" id="cardano-sign-transaction"></a>

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

```typescript
const response = await HardwareSDK.confluxSignTransaction(connectId, deviceId, params)
```

### Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `ConfluxTransaction` type
  * `to` - _required  `string` _ of address hex string
  * `value` - _required  `string` _ of amount convert to BigInt hex string
  * `gasLimit` - _required  `string` _ of gas limit convert to BigInt hex string
  * `gasPrice` - _required  `string` _ of gas price convert to BigInt hex string
  * `nonce` - _required  `string` _ of nonce convert to BigInt hex string
  * `epochHeight` - _required  `string` _ of epoch height convert to BigInt hex string
  * `storageLimit` - _required  `string` _ of storage limit convert to BigInt hex string
  * `chainId` - _optional  `number` _ of the id of the chain
  * `data` - _optional  `string` _ of hex data
  * `data_initial_chunk` - _optional  `string` _ of data initial chunk
  * `data_length` - _optional  `string` _ of data length

### Example

```typescript
HardwareSDK.confluxSignMessage(connectId, deviceId, {
    path: "m/44'/503'/0'/0'/0",
    transaction: {
        to: "0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8",
        value: "0xf4240",
        data: "0x01",
        chainId: "1",
        nonce: "0x00",
        epochHeight: "0x00",
        gasPrice: "0xbebc200",
        gasLimit: "0x5208",
        storageLimit: "0x5208",
    }
});
```

Result

```typescript
{
    success: true,
    payload: {
        v: string;
        r: string;
        s: string;
    }
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
