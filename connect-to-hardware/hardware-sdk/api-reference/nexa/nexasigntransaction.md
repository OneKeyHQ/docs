# nexaSignTransaction

### Use requirement

* Firmware version required
  * Touch: 4.4.0
  * Classic/Mini: 3.2.0

## Nexa: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nexaSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `inputs` - _required_ `Array` transaction inputs
  * `path` — _required_ `string | Array<number>` minimum length is `3`. read more
  * `message` - _required_ `string` type of serialized transaction string.
  * `prefix` - _optional_ `string` Address prefix. Default is set to `nexatest`
  * `scheme` - _optional_ `string` Encryption algorithm mode. Default is set to `schnorr`
* `outputs` - _required_ `Array` transaction outputs
  * `satoshis` - _required_ `number | string` transaction amount
  * `script` - _required_ `string` transaction script
  *   `scriptVersion` - _required_ `number` transaction scriptVersion



### Examples

```typescript
const response = await HardwareSDK.nexaSignTransaction(
  connectId,
  deviceId,
  {
    inputs: [
      {
         path: "m/44'/29223'/0'/0/0",
         message: '000578c6c76f10156fbc7ee4a8faa7a4e92b6adadc978abf66ae70f13a03b75d36cd7a6acc0967cc9f2f632f585cb7b4297873858c23233792767fd4ae662ec1093bb13029ce7b1f559ef5e747fcac439f1455a2ec7c5f09b72290795e70665044026cad0dba749a112e0d2ea420fa68e0218453db6bb0744e44eb51edc76af8bb6871190000000000',
         prefix: 'nexatest',
      }
    ],
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
