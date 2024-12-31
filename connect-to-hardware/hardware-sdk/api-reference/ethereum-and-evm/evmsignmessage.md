# evmSignMessage

### Ethereum: sign message

Asks device to sign a message using the private key derived by given BIP32 path.

* **Follows EIP-191 personal\_sign standard**
* **Not for EIP-712 typed data signing.**
* **Use** [**evmSignTypedData**](evmsigntypeddata.md) **to signing EIP-712 (v3 and v4)**

```typescript
const result = await HardwareSDK.evmSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `messageHex` - _required_ `string` message to sign in hex text
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain. [Reference](https://github.com/ethereum-lists/chains/tree/master/_data/chains).&#x20;

### Example

```typescript
// Original message
const message = "Hello OneKey";

// Convert to hex
const messageHex = Buffer.from(message).toString('hex');

HardwareSDK.evmSignMessage(connectId, deviceId, {
    path: "m/44'/60'/0'",
    messageHex: messageHex,
    chainId: 1
});
```

#### Result

```typescript
{
    success: true,
    payload: {
        address: string,
        signature: string,
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
