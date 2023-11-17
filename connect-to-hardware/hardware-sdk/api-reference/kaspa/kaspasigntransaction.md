# kaspaSignTransaction

### Use requirement

* Firmware version required
  * Touch: 4.3.0
  * Classic/Mini: 3.0.0

## Kaspa: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.kaspaSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `version` — _required_ `number` transaction version.
* `inputs` - _required_ `Array` of [KaspaSignInputParams](https://github.com/OneKeyHQ/hardware-js-sdk/blob/269ee367141ae2186a9a63d1d89e9f4c70428623/packages/core/src/types/api/kaspaSignTransaction.ts#L9C1-L19C3).
* `outputs` - _required_ `Array` of [KaspaSignOutputParams](https://github.com/OneKeyHQ/hardware-js-sdk/blob/269ee367141ae2186a9a63d1d89e9f4c70428623/packages/core/src/types/api/kaspaSignTransaction.ts#L21C1-L26C1).
* `lockTime` - _required_ `number`&#x20;
* `sigHashType` - _required_ `number`&#x20;
  * ```
    SIGHASH_ALL = 0x01,
    SIGHASH_NONE = 0x02,
    SIGHASH_SINGLE = 0x03,
    SIGHASH_FORKID = 0x40,
    SIGHASH_ANYONECANPAY = 0x80,
    ```
* `sigOpCount` - _optional_ `number`&#x20;
* `subNetworkID` - _optional_ `string`&#x20;
* `prefix` - _optional_ `string` Address prefix. Default is set to `kaspa`
* `scheme` - _optional_ `string` Encryption algorithm mode. Default is set to `schnorr`



### Examples

```typescript
const response = await HardwareSDK.kaspaSignTransaction(
  connectId,
  deviceId,
  {
    version: 0,
    lockTime: '0',
    sigHashType: 0x1,
    sigOpCount: 1,
    subNetworkID: "00000000000000000000000000000000",
    prefix: "prefix",
    scheme: "schnorr",
    inputs: [
      {
        outputIndex: 1,
        path: "m/44'/111111'/0'/0/0",
        prevTxId: '1f226507807ff7dc5a7f8f2dec353fffc9dacc2645d8aecd02e5046907e3e2b2',
        sequenceNumber: '0',
        sigOpCount: 1,
        output: {
          satoshis: '990096458',
          script: '207afdae557e69c0040fd4135adffc60f9486fb21f4cbae233fd6db3e84ba47c55ac',
        },
      },
    ],
    outputs: [
      {
        satoshis: '100000000',
        script: '205ca3a7530284e5c5e472544edd6002c3afeb8c8f84d3a728fad255a4872753fbac',
        scriptVersion: 0,
      },
      {
        satoshis: '890094182',
        script: '207afdae557e69c0040fd4135adffc60f9486fb21f4cbae233fd6db3e84ba47c55ac',
        scriptVersion: 0,
      },
    ]
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    index: 0,
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
