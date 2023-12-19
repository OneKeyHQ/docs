# nostrEncryptMessage

### Use requirement

* Firmware version required
  * Touch: 4.6.0
  * Classic/Mini: 3.5.0
  * Classic1s: 3.6.0

## Nostr: Encrypt Message

Asks device to encrypt message using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nostrEncryptMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `pubkey` — _required_ `string` public key .
* `plaintext` — _required_ `string` messages that need to be encrypted.
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true` .

### Examples

```typescript
const response = await HardwareSDK.nostrEncryptMessage(
  connectId,
  deviceId,
  {
     path: "m/44'/1237'/0'/0/0",
     pubkey: '2118c65161c7d68b4bdbe1374f658532670057ab1bb0c99937d0ff7cff45cb5e',
     plaintext: 'Hello world',
     showOnOneKey: true,
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    path: string;
    pubkey: string;
    plaintext: string;
    encryptedMessage: string;
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
