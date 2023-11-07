# starcoinVerifyMessage

## Starcoin: verify message

Asks device to verify a message using the signer address and signature.

ES6

```typescript
const result = await HardwareSDK.starcoinVerifyMessage(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

* `publicKey` - _required_ `string` signer publicKey.
* `messageHex` - _required_ `string` signed message in hex text
* `signature` - _required_ `string` signature in hexadecimal format.

## Example

```typescript
HardwareSDK.starcoinVerifyMessage(connectId, deviceId, {
    publicKey: "cf90aea8962229869bcba527f0cf0de73ad4c22fe27ad2875007e967db7056f5",
    message: "0x6578616d706c65206d657373616765",
    signature: "447d2c3131c32d176106482d8fc780d933f13b37b0f06beee27de5e73a945e8c807f7764d269a196306f53383a26ab632913f4076457e3230b9defafe1c6ba0b",
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
