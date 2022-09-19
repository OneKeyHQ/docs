# btcSignMessage

## Bitcoin: sign message

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.signMessage(connectId, deviceId, params);
```

## Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `messageHex` - _required_ `string` message from hex
* `coin` - _optional_ `string` Determines network definition specified in coins.json file. Coin `shortcut`, `name` or `label` can be used. If `coin` is not set API will try to get network definition from `path`.

### Example

```typescript
HardwareSDK.signMessage(connectId, deviceId, {
    path: "m/44'/0'/0'",
    messageHex: "6578616d706c65206d657373616765"
});
```

### Result

```typescript
{
    success: true,
    payload: {
        address: string,   // signer address
        signature: string, // signature in base64 format
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
