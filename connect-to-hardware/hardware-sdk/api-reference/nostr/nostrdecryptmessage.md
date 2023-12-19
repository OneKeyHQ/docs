# nostrDecryptMessage

### Use requirement

* Firmware version required
  * Touch: 4.6.0
  * Classic/Mini: 3.5.0
  * Classic1s: 3.6.0

## Nostr: Decrypt Message

Asks device to decrypt message using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nostrDecryptMessage(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `pubkey` — _required_ `string` public key .
* `ciphertext` — _required_ `string` encrypted message.
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true` .



### Examples

```typescript
const response = await HardwareSDK.nostrDecryptMessage(
  connectId,
  deviceId,
  {
     path: "m/44'/1237'/0'/0/0",
     pubkey: '2118c65161c7d68b4bdbe1374f658532670057ab1bb0c99937d0ff7cff45cb5e',
     ciphertext: 'VpWFJ7JDFv16jL7pBZ1shw==?iv=$1tPpwGD3Ic1RTVXJx1ZG7Q==',
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
    ciphertext: string;
    decryptedMessage: string;
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
