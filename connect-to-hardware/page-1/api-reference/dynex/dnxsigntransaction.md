# dnxSignTransaction

### Dynex: sign transaction

Signs the given Dynex transaction with the private key derived by given BIP32 path. User is asked to confirm the transaction details on OneKey.

```typescript
const result = await HardwareSDK.dnxSignTransaction(connectId, deviceId, params);
```

#### Use requirement

* Firmware version required
  * classic: 3.8.0

#### Parameters

connectId: `string`

* Unique identifier for the connection

deviceId: `string`

* Unique identifier for the hardware device

params: `object`

* path: `string` - BIP32 path (e.g. "m/44'/0'/0'/0/0") \[required]
* inputs: `array` - Array of transaction inputs \[required]
  * prevIndex: `number` - Previous output index
  * globalIndex: `number` - Global index
  * txPubkey: `string` - Transaction public key
  * prevOutPubkey: `string` - Previous output public key
  * amount: `string` - Input amount
* toAddress: `string` - Destination address \[required]
* amount: `string` - Transaction amount \[required]
* fee: `string` - Transaction fee \[required]
* paymentIdHex: `string` - Optional payment ID (must be 32 bytes)

#### Example

```typescript
const response = await HardwareSDK.dnxSignTransaction('connect1', 'device1', {
    path: "m/44'/0'/0'/0/0",
    inputs: [{
        prevIndex: 0,
        globalIndex: 323,
        txPubkey: "0d708b68003ae5fee4c9f9aad56a8cfdb0a5dcbcdd2e3fc18eb813349457a78c",
        prevOutPubkey: "ff4df9a9fc83e48f2c0242c4d94f3906546a889950bccd8ba4392f3e7886c3a1",
        amount: "1000000000"
    }],
    toAddress: "XwmxTF8FxAy2s5cvtS62oSGxY3fzvDcgo2CiJ6rrpz9te68sApSDs3LQihubFtpsfT5z6NYHZzMKUjavNpTkW46i2dYgHBULG",
    amount: "1000000000",
    fee: "100000",
});
```

#### Returns

Success response:

```typescript
{
    success: true,
    payload: {
        path: string,              // BIP32 path used for signing
        txKey: {
            ephemeralTxSecKey: string,  // Transaction secret key
            ephemeralTxPubKey: string   // Transaction public key
        },
        computedKeyImages: string[],    // Array of computed key images
        signatures: string[],           // Array of signatures
        outputKeys: string[]            // Array of output keys
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

####
