# algoSignTransaction

## Algorand: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.algoSignTransaction(connectId, deviceId, params);
```

### Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ `string` type of serialized transaction string.

### Examples

```typescript
const response = await HardwareSDK.algoSignTransaction(
  connectId,
  deviceId,
  {
    path: "m/44'/283'/0'/0'/0'",
    rawTx: "4301355cc18d85809872bcbd63cb6ea5ac3c2814a1bacf2e50d8ec62367211917b79ecd1f1a98fa0d793d7cb92ebd9a479dc6aba0ae8570253aa87c0da32db5ed2bd401f3bbee52c2bc55761fd8486fae2e28f46499282f4267b8b90fc8c1cc97bb659b6cc927f2ec1701ef2928ddb84759ba5c557f549db"
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    path: "m/44'/283'/0'/0'/0'",
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
