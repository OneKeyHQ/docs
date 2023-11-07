# stellarSignTransaction

## Stellar: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.stellarSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../../common-params.md)

* `path` — _required_ `string | Array<number>` **length is `3`, Here's a special chain.** read more
* `networkPassphrase` - _required_ `string` network passphrase
* `transaction` - _required_ `Object` type of StellarTransaction

#### StellarTransaction Type

```typescript
export type StellarTransaction = {
  source: string;
  fee: number;
  sequence: string | number;
  timebounds?: {
    minTime: number;
    maxTime: number;
  };
  memo?: {
    type: 0 | 1 | 2 | 3 | 4;
    id?: string;
    text?: string;
    hash?: string | Buffer;
  };
  operations: StellarOperation[];
};
```

### Examples

```typescript
const response = await HardwareSDK.aptosSignTransaction(
  connectId,
  deviceId,
  {
    path: "m/44'/148'/0'",
    networkPassphrase: "Test SDF Network ; September 2015",
    transaction: {
        source: "GAXSFOOGF4ELO5HT5PTN23T5XE6D5QWL3YBHBH2HWOFEJNYYMRJENJR",
        fee: 200,
        sequence: 4238497296,
        memo: {
            type: 0,
        },
        operations: [
            {
                type: "payment",
                source: "GAXSFOOGF4ELO5HT5PTN23T5XE6D5QWL3YBHBH2HWOFEJNYYMRJENJR",
                destination: "GAXSFOOGF4ELO5HT5PTN23T5XE6D5QWL3YBHBH2HWOFEJNYYMRJENJR",
                amount: "10000"
            }
        ]
    }
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
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
