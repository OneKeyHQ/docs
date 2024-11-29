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
    rawTx: "b00100001c000000200000006e00000072000000ce0000009c010000000000000200000071a7ba8fc96349fea0ed3a5c47992e3b4084b031a42264a018e0072e8172e46c0000000001c7813f6a415144643970c2e88e0bb6ca6a8edc5dd7c1022746f628284a9936d50000000000000000000200000000000000000000003ccb539e56ce1acaeb53db6a6ce939132c5f462e7eb686556f3ba308b4c402080100000000000000000000004f4803ed365e368fb6ac0048961883edf1a98daf20a3d73a54d26e4aa601d7d300000000ce0000000c0000006d000000610000001000000018000000610000000002648901000000490000001000000030000000310000009bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce801140000006655cba91b01e4f8e3ecb40d1fb6241a9033115a610000001000000018000000610000009651104802000000490000001000000030000000310000009bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce80114000000fb36a5944e2626089db2f37e9a5cb4a6eff55c82140000000c000000100000000000000000000000", // Raw transaction hex
    witnessHex: "55000000100000005500000055000000410000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000", // Witness data hex
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

