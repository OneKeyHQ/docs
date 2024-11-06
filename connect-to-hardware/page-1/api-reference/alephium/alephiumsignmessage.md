# alephiumSignMessage

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### Alephium: sign message

Sign a message using the private key derived by given BIP32 path. User needs to confirm the action on OneKey device.

```typescript
const result = await HardwareSDK.alephiumSignMessage(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
    messageHex: "68656c6c6f",  // "Hello Alephium" in hex
    messageType: "alephium"
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is 3
* `messageHex` — _required_ `string` message to sign in hexadecimal format
* `messageType` — _optional_ `string` type of message signing algorithm (default: "alephium")

#### Example

```typescript
const response = await HardwareSDK.alephiumSignMessage(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
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
