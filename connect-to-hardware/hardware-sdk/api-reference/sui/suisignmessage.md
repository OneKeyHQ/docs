# suiSignMessage

### Use requirement

* Firmware version required
  * Touch: 4.6.0
  * Classic/Mini: 3.4.0

### Sui: sign message

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.suiSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `messageHex` - _required_ `string` message to sign in hex text

### Example

```typescript
// Original message
const message = "Hello World";

// Convert to hex
const messageHex = Buffer.from(message).toString('hex');

HardwareSDK.suiSignMessage(connectId, deviceId, {
    path: "m/44'/784'/0'/0'/0'",
    messageHex,
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
