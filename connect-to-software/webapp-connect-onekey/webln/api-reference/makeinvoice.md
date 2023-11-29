# makeInvoice

Request that the user creates an invoice to be used by the web app. This will return a [BOLT-11](https://github.com/lightningnetwork/lightning-rfc/blob/master/11-payment-encoding.md) invoice. Invoices can be requested in a few forms:

* By specifying an explicit `amount`, the user's provider should enforce that the user generate an invoice with a specific amount
* By specifying a `minimumAmount` and / or `maximumAmount`, the user's provider should enforce that the user generate an invoice with an amount field constrained by that amount
* When an explicit `amount` is _not_ set, the us

Amounts are denominated in satoshis. For large amounts, it's recommended you use a big number library such as [bn.js](https://www.npmjs.com/package/bn.js) or [big.js](https://www.npmjs.com/package/big.js) as Javascript only supports 32 bit integers.

### Method

```typescript
async function makeInvoice(args: RequestInvoiceArgs): RequestInvoiceResponse
```

### Params

* `event` — _required_ `object`
  * `amount` — _optional_ `string|number`  the satoshis to send
  * `defaultAmount` — _optional_ `string|number`
  * `minimumAmount` — _optional_ `string|number`
  * `maximumAmount` — _optional_ `string|number`
  * `defaultMemo` — _optional_ `string`

```typescript
export interface RequestInvoiceArgs {
  amount?: string | number; // unit is sats
  defaultAmount?: string | number; // unit is sats
  minimumAmount?: string | number; // unit is sats
  maximumAmount?: string | number; // unit is sats
  defaultMemo?: string;
}
```

### Response

```typescript
export interface RequestInvoiceResponse {
  paymentRequest: string;
  paymentHash: string;
  rHash: string;
}
```

### Example

```typescript
await window.$onekey?.webln.enable();
const invoice = await window.$onekey?.webln.makeInvoice({
  amount: 1000,
});
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/ZEwMKvw" %}
