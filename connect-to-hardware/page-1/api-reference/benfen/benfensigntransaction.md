# benfenSignTransaction



### Benfen: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.benfenSignTransaction(connectId, deviceId, {
  path: "m/44'/728'/0'/0'/0'",
  rawTx: "0x00000000000200088096980000000000002017f3a9bd36da0639153d3c38032217ea298eb1991e0a62cc5924e2dd712937350202000101000001010300000000010100b4ced58018b75d7ba72a10fa97c09b7bf66533ff104bf9db1bfdb004b17d8eaa0183006eb3c5499c3cb4b022f20955f387312ed312389c552fa39e35d6423d0c74f785b7000000000020497f41fdfb22d2ae32111dcdc21c16d27f48f47b7ca7aba240b684db5ca74c0fb4ced58018b75d7ba72a10fa97c09b7bf66533ff104bf9db1bfdb004b17d8eaa640000000000000060ad38000000000000", // Raw transaction hex
  coinType: "0x2::bfc::BFC"
});
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ `string` type of serialized transaction string in hex format
* `coinType` - _optional_ `string` coin type identifier

#### Returns

* success: boolean - True if successful
* payload:
  * signature: string - Transaction signature in hex format
  * path: string - BIP32 path used
  * coinType?: string - Coin type if provided,(e.g. 0x2::bfc::BFC,  0xc8::busd::BUSD)

#### Example

```typescript
const params = {
  path: "m/44'/728'/0'/0'/0'",
  rawTx: "0x00000000000200088096980000000000002017f3a9bd36da0639153d3c38032217ea298eb1991e0a62cc5924e2dd712937350202000101000001010300000000010100b4ced58018b75d7ba72a10fa97c09b7bf66533ff104bf9db1bfdb004b17d8eaa0183006eb3c5499c3cb4b022f20955f387312ed312389c552fa39e35d6423d0c74f785b7000000000020497f41fdfb22d2ae32111dcdc21c16d27f48f47b7ca7aba240b684db5ca74c0fb4ced58018b75d7ba72a10fa97c09b7bf66533ff104bf9db1bfdb004b17d8eaa640000000000000060ad38000000000000", 
  // Raw transaction hex
  coinType: "0x2::bfc::BFC"
};

const response = await HardwareSDK.benfenSignTransaction(connectId, deviceId, params);
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
