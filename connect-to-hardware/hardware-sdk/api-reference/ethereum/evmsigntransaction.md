# evmSignTransaction

### Ethereum: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.evmSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `transaction` - _required_ `Object` type of `EthereumTransactionEIP1559|EthereumSignTransaction` "0x" prefix for each field is optional

### Examples

#### EIP1559 ([after The London Upgrade](https://ethereum.org/en/developers/docs/gas/#post-london))

If both parameters `maxFeePerGas` and `maxPriorityFeePerGas` are defined, transaction will be signed as the new type introduced in [EIP1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md).

<pre class="language-typescript"><code class="lang-typescript"><strong>HardwareSDK.evmSignTransaction(connectId, deviceId, {
</strong>    path: "m/44'/60'/0'",
    transaction: {
        to: '0xd0d6d6c5fe4a677d343cc433536bb717bae167dd',
        value: '0xf4240',
        data: '0xa',
        chainId: 1,
        nonce: '0x0',
        maxFeePerGas: '0x14',
        maxPriorityFeePerGas: '0x0',
        gasLimit: '0x14',
    }
});
</code></pre>

Legacy

```typescript
HardwareSDK.evmSignTransaction(connectId, deviceId, {
    path: "m/44'/60'/0'",
    transaction: {
        to: "0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8",
        value: "0xf4240",
        data: "0x01",
        chainId: 1,
        nonce: "0x0",
        gasLimit: "0x5208",
        gasPrice: "0xbebc200"
    }
});
```

Result

```typescript
{
    success: true,
    payload: {
        v: string, // hexadecimal string with "0x" prefix
        r: string, // hexadecimal string with "0x" prefix
        s: string, // hexadecimal string with "0x" prefix
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
