# cardanoSignMessage

## Use requirement

* Firmware version required
  * Touch: 4.1.0
  * Classic/Mini: 3.0.0

## Cardano: sign message <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.cardanoSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `message` - _required_ `string` message to sign in hex text
* `networkId` - _required_ `Integer` 1 for Mainnet, 0 for Testnet
* `derivationType` — _optional_ `CardanoDerivationType` enum. determines used derivation type. Default is set to ICARUS=1

### Example

```typescript
// Original message
const message = "Hello World";

// Convert to hex
const messageHex = Buffer.from(message).toString('hex');

HardwareSDK.cardanoSignMessage(connectId, deviceId, {
    path: "m/1852'/1815'/0'/0/0",
    message: messageHex,
    networkId: 1
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
        key: string;
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
