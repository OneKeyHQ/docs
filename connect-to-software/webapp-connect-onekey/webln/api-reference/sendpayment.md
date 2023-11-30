# sendPayment

Request that the user sends a payment for an invoice. The application needs to provide a BOLT-11 invoice

### Method

```typescript
async function sendPayment(paymentRequest: string): SendPaymentResponse
```

* `paymentRequest`:  The invoice you'd like the user to pay (lnbc...)

### Response

```typescript
interface SendPaymentResponse {
  preimage: string;
}
```

### Example

```typescript
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

const invoice = "lnbc100xxxx";
await provider.enable();
const info = await provider.sendPayment(invoice);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/OJdomvO" %}
