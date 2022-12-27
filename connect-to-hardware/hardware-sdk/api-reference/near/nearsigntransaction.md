# nearSignTransaction

## Near: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nearSignTransaction(connectId, deviceId, params);
```

### Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ `string` type of serialized transaction string.

### Examples

```typescript
const response = await HardwareSDK.nearSignTransaction(
  connectId,
  deviceId,
  {
    path: "m/44'/397'/0'/0/0",
    rawTx: "400000003630323130393034396561313633656561326634386365616238303634363932373538323730323938333863666163303865633463363330303431353639613600602109049ea163eea2f48ceab806469275827029838cfac08ec4c630041569a644255eea2d4200004000000034376464643364346536393632343535386266313135643438313763336566303861386264393864313832666466666637373465353065643937626637626437d2a5c8e15cadc0476f5f07a02b2a3b9c1699847996b1bc55142b881a3ff1accd010000000301000000000000000000000000000000"
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
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
