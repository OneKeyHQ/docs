# evmVerifyMessage

## Ethereum: verify message

Asks device to verify a message using the signer address and signature.

```typescript
const result = await HardwareSDK.evmVerifyMessage(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

* `address` - _required_ `string` signer address. "0x" prefix is optional
* `messageHex` - _required_ `string` signed message in hex text
* `signature` - _required_ `string` signature in hexadecimal format. "0x" prefix is optional
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain. [Reference](https://github.com/ethereum-lists/chains/tree/master/\_data/chains).&#x20;

## Example

```typescript
HardwareSDK.evmVerifyMessage(connectId, deviceId, {
    address: "0xdA0b608bdb1a4A154325C854607c68950b4F1a34",
    message: "Example message",
    signature: "11dc86c631ef5d9388c5e245501d571b864af1a717cbbb3ca1f6dacbf330742957242aa52b36bbe7bb46dce6ff0ead0548cc5a5ce76d0aaed166fd40cb3fc6e51c",
    chainId: 1
});
```

## Result

```typescript
{
    success: true,
    payload: {
        message: "Message verified"
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
