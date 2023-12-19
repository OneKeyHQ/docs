# nostrSignSchnorr

### Use requirement

* Firmware version required
  * Touch: 4.6.0
  * Classic/Mini: 3.5.0
  * Classic1s: 3.6.0

## Nostr: Sign Schnorr

Asks device to sign given schnorr transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nostrSignSchnorr(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `hash` — _required_ `string` schnorr hash.



### Examples

```typescript
const response = await HardwareSDK.nostrSignSchnorr(
  connectId,
  deviceId,
  {
    path: "m/44'/1237'/0'/0/0",
    hash: '2118c65161c7d68b4bdbe1374f658532670057ab1bb0c99937d0ff7cff45cb5e',
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    path: string;
    rawHash: string,
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
