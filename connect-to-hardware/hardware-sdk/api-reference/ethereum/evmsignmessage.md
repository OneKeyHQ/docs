# evmSignMessage

### Ethereum: sign message

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.evmSignMessage(connectId, deviceId, params);
```

### Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `messageHex` - _required_ `string` message to sign in hex text

### Example

```typescript
HardwareSDK.evmSignMessage(connectId, deviceId, {
    path: "m/44'/60'/0'",
    messageHex: "6578616d706c65206d657373616765"
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
