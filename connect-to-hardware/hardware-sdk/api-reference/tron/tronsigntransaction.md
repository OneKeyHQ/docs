# tronSignTransaction

### TRON: Sign transaction <a href="#cardano-sign-transaction" id="cardano-sign-transaction"></a>

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

```typescript
const response = await HardwareSDK.tronSignTransaction(connectId, deviceId, params)
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `TronTransaction` type

#### TronTransaction Type

```typescript
type TronTransaction = {
  refBlockBytes: string;
  refBlockHash: string;
  expiration: number;
  data?: string;
  contract: TronTransactionContract;
  timestamp: number;
  feeLimit?: number;
};

type TronTransactionContract = {
  transferContract?: TronTransferContract;
  triggerSmartContract?: TronTriggerSmartContract;
};

type TronTransferContract = {
  toAddress?: string;
  amount?: UintType;
};

type TronTriggerSmartContract = {
  contractAddress?: string;
  callValue?: number;
  data?: string;
  callTokenValue?: number;
  assetId?: number;
};

```

### Example

```typescript
HardwareSDK.tronSignTransaction(connectId, deviceId, {
    path: "m/44'/195'/0'/0'/0",
    transaction: {
        refBlockBytes: 'ddf1',
        refBlockHash: 'd04764f22469a0b8',
        timestamp: 1655692083406,
        expiration: 1655692140000,
        contract: {
            transferContract: {
                toAddress: 'TXrs7yxQLNzig7J9EbKhoEiUp6kWpdWKnD',
                amount: 100,
            },
        }
    }
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
        serialized_tx: string;
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
