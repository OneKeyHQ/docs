# scdoSignMessage

#### Use requirement

* Firmware version required
  * Touch: 4.10.0

#### SCDO: sign message

Signs a message using the private key derived by given BIP32 path.

```typescript
// Original message
const message = "Hello World";

// Convert to hex
const messageHex = Buffer.from(message).toString('hex');

const response = await HardwareSDK.scdoSignMessage(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    messageHex: messageHex
});
```

**Params**

Optional common params

* `path` — _required_ `string | Array<number>` minimum length is 3. read more
* `messageHex` — _required_ `string` message to sign in hex format

**Example**

```typescript
// Original message
const message = "Hello World";

// Convert to hex
const messageHex = Buffer.from(message).toString('hex');

const response = await HardwareSDK.scdoSignMessage(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    messageHex,
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string,
        address: string
    }
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number  // error code
    }
}
```
