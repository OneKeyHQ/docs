# nervosSignTransaction

### Nervos: sign transaction

Signs the given Nervos transaction with the private key derived by given BIP32 path. User is asked to confirm the transaction details on OneKey.

```typescript
const result = await HardwareSDK.nervosSignTransaction(connectId, deviceId, params);
```

#### Use requirement

* Firmware version required
  * Touch: 4.9.0
  * mini: 3.7.0

#### Parameters

connectId: `string`

* Unique identifier for the connection

deviceId: `string`

* Unique identifier for the hardware device

params: `object`

* path: `string` - BIP32 path (e.g. "m/44'/309'/0'/0/0") \[required]
* rawTx: `string` - Raw transaction data in hex format \[required]
* witnessHex: `string` - Witness data in hex format \[required]
* network: `string` - Network name (e.g. "ckb", "ckb\_testnet") \[required]

#### Example

```typescript
const response = await HardwareSDK.nervosSignTransaction('connect1', 'device1', {
    path: "m/44'/309'/0'/0/0",
    rawTx: "0x...", // Raw transaction hex
    witnessHex: "0x...", // Witness data hex
    network: "ckb"
});
```

#### Returns

Success response:

```typescript
{
    success: true,
    payload: {
        signature: string,  // Signature in hex format
        address: string,   // Address that signed the transaction
        path: string      // BIP32 path used for signing
    }
}
```

Error response:

```typescript
{
    success: false,
    payload: {
        error: string,
        code: number
    }
}
```

