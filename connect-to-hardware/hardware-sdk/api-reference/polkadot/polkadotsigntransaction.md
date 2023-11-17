# polkadotSignTransaction

### Use requirement

* Firmware version required
  * Touch: 4.3.0
  * Classic/Mini: 3.0.0

## Polkadot: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.polkadotSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `network` - _required_ `string` Network name, Default is set to `polkadot`
* `rawTx` - _required_ `string` type of serialized transaction string.

### Examples

```typescript
const response = await HardwareSDK.polkadotSignTransaction(
  connectId,
  deviceId,
  {
    path: "m/44'/354'/0'/0'/0'",
    rawTx: "040300388671dd546ba19495211c8cdc200600f9798cf078fca0fb4749ebbc0579c12a0700e40b5402350128009a24000012000000e143f23803ac50e8f6f8e62695d1ce9e4e1d68aa36c1cd2cfd15340213f3423ee356a704dbe75762aaae87b0f4702aafd09e67eed5d09a3a4b0e7da6c37b6f54"
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    path: "m/44'/354'/0'/0'/0'",
    signature: "signed data"
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
