# tonSignProof

## TON: sign message <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.tonSignMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../../../hardware-sdk/api-reference/common-params.md)

* `address_n` - _required_ `Array<number>`  BIP-32 path to derive the key from master node
* `appdomain` - _required_ `string` dapp address
* expireAt - _required_ `number` message expiration time

### Example

```typescript
HardwareSDK.tronSignMessage(connectId, deviceId, {
    path: "m/44'/607'/0'",
    appdomain: "onekey.so",
    expireAt: 1728713831896,
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
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
