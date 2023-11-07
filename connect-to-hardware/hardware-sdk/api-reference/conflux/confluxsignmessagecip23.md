# confluxSignMessageCIP23

## Use requirement

* Firmware version required
  * Touch: 3.0.0
  * Classic/Mini: 2.4.0

## Conflux: sign message CIP23 <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.This method follows the [CIP23](https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-23.md) specification implementation

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `messageHash` - _required_ `string` message to sign in hex text
* `domainHash` - _required_ `string` hex-encoded 32-byte hash of the CIP-23 domain.

### Example

```typescript
HardwareSDK.confluxSignMessage(connectId, deviceId, {
    path: "m/44'/503'/0'/0/0",
    messageHash: "0x07bc1c4f3268fc74b60587e9bb7e01e38a7d8a9a3f51202bf25332aa2c75c64"
    domainHash: "7c872d109a4e735dc1886c72af47e9b4888a1507557e0f49c85b570019163373"
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
