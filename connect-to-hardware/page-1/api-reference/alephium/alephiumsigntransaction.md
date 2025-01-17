# alephiumSignTransaction

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### Alephium: sign transaction

Sign Alephium transaction using the private key derived by given BIP32 path. User needs to confirm the action on OneKey device.

```typescript
const result = await HardwareSDK.alephiumSignTransaction(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
    rawTx: "00010080004e20c1174876e80001f3bf9d774d87e65e79a72d5482da5ef35aac8d0b930b394bdd67586930f6ae7b4fd8362b0003f6bd3137b3cadbad59a73527c3b8e26429bba093722a59639f22af9d2adb477302c40de0b6b3a7640000001e98167f559360e002c3f3ceb3cc95c1dad848403f0296c76439093d2b9879ac00000000000000000000c43c7a13049ed60800001e98167f559360e002c3f3ceb3cc95c1dad848403f0296c76439093d2b9879ac00000000000000000000"
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is 3
* `rawTx` — _required_ `string` raw transaction in hex format



#### Example

```typescript
const response = await HardwareSDK.alephiumSignTransaction(connectId, deviceId, {
    path: "m/44'/1234'/0'/0/0",
    rawTx: "0x0123...",
    scriptOpt: "p2pkh"
});
```

Result

```typescript
{
  success: true,
  payload: {
    signature: string,
    address: string.
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
