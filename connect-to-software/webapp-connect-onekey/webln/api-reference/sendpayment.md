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
const invoice = "lnbc100xxxx";
await window.webln.enable();
const info = await window.webln.sendPayment(invoice);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/OJdomvO" %}
