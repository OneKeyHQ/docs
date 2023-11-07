# xrpSignTransaction

### Ripple: Sign transaction <a href="#cardano-sign-transaction" id="cardano-sign-transaction"></a>

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

```typescript
const response = await HardwareSDK.xrpSignTransaction(connectId, deviceId, params)
```

### Params

[**Optional common params**](../../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `XrpTransaction` type

#### XrpTransaction Type

```typescript
type XrpTransaction = {
    fee: string;
    flags?: number;
    sequence: number;
    maxLedgerVersion?: number; // Proto: "last_ledger_sequence"
    payment: RipplePayment;
}

type XrpPayment = {
    amount: string;
    destination: string;
    destinationTag?: number;
}
```

### Example

```typescript
HardwareSDK.xrpSignTransaction(connectId, deviceId, {
    path: "m/44'/144'/0'/0'/0",
    transaction: {
        fee: 12,
        flags: 0,
        sequence: 32841006,
        maxLedgerVersion: 32841630,
        payment: {
            amount: 1000000,
            destination: 'rwgumKP89VhMrJ4dRkGVS4tafRfAmZmKf8',
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
        serializedTx: string;
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
