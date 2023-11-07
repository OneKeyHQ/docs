# tronSignMessage

## TRON: sign message <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.tronSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `messageHex` - _required_ `string` message to sign in hex text

### Example

```typescript
HardwareSDK.tronSignMessage(connectId, deviceId, {
    path: "m/44'/195'/0'/0/0",
    message: "0x6578616d706c65206d657373616765"
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
        address: string;
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
