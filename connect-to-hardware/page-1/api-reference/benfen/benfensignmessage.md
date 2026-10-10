# benfenSignMessage

**Requirements**

* Minimum firmware versions
  * Pro: 4.12.0

### Benfen: sign message

Sign a message using the private key derived from the specified BIP32 path. User needs to confirm the action on OneKey device.

```typescript
const result = await HardwareSDK.benfenSignMessage(connectId, deviceId,{
  path: "m/44'/728'/0'/0'/0'",
  messageHex: "68656c6c6f" // "hello" in hex
});
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` SDK minimum length is `3`. [Path format](../../../hardware-sdk/api-reference/path.md)
* `messageHex` - _required_ `string` Message to sign in hex format

#### Returns

* `success` - _boolean_ True if successful
* `payload`
  * `signature` - _string_ Signature in hex format
  * `address` - _string_ BFC format address used for signing

Example:

```typescript
const params = {
  path: "m/44'/728'/0'/0'/0'",
  messageHex: "68656c6c6f" // "hello" in hex
};

const response = await HardwareSDK.benfenSignMessage(connectId, deviceId, params);
```

Result

```typescript
{
  success: true,
  payload: {
    signature: string,
    address: string,
  }
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code?: string | number // error code
    }
}
```
