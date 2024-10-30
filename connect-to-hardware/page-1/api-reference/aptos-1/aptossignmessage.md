# alephiumSignMessage

## Use requirement

* Firmware version required
  * Touch: 4.10.0

## Aptos: sign message <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.alephiumSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../../../hardware-sdk/api-reference/common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `messageHex` - _optional  `hexString`_ message to sign in hex text
* `messageType` - _required_ `string` 'alephium' or 'utf-8'

### Example

```typescript
HardwareSDK.alephiumSignMessage(connectId, deviceId, {
    path: "m/44'/1234'/0'/0'/0'",
    messageHex: "68656c6c6f",
    messageType: "alephium"
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
