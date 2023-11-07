# btcVerifyMessage

## Bitcoin: verify message

Asks device to verify a message using the signer address and signature.

```typescript
const result = await HardwareSDK.btcVerifyMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `address` - _required_ `string` signer address,
* `messageHex` - _required_ `string` signed message from hex,
* `signature` - _required_ `string` signature in base64 format,
* `coin` - _required_ `string` Determines network definition specified in coins.json file. Coin `shortcut`, `name` or `label` can be used.

### Example

```typescript
HardwareSDK.btcVerifyMessage(connectId, deviceId, {
    address: "3BD8TL6iShVzizQzvo789SuynEKGpLTms9",
    messageHex: "6578616d706c65206d657373616765"
    signature: "JO7vL3tOB1qQyfSeIVLvdEw9G1tCvL+lNj78XDAVM4t6UptADs3kXDTO2+2ZeEOLFL4/+wm+BBdSpo3kb3Cnsas=",
    coin: "btc"
});
```

### Result

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
