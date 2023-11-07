# suiSignTransaction

## Sui: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.suiSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ `string` type of serialized transaction string.

### Examples

```typescript
const response = await HardwareSDK.suiSignTransaction(
  connectId,
  deviceId,
  {
    path: "m/44'/784'/0'/0'/0'",
    rawTx: "5472616e73616374696f6e446174613a3a0005012da3a446433920a4a290f834710575bd943631de0200000000000000207a7b5943ca4bfa227033fa7c6ddb452c7f7f8a18f73f1058b4ac07a14d22e0a5017cb14a7a318219299f008f31e977808c96e372a60140420f0000000000c68311c708918b26fa85b4ded86977baf81086872da3a446433920a4a290f834710575bd943631de0200000000000000207a7b5943ca4bfa227033fa7c6ddb452c7f7f8a18f73f1058b4ac07a14d22e0a501000000000000007a00000000000000"
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    path: "m/44'/784'/0'/0'/0'",
    public_key: string,
    signature: string.
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
