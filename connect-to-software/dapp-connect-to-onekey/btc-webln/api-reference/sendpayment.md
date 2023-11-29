# sendPayment

Request that the user sends a payment for an invoice. The application needs to provide a BOLT-11 invoice

### Method

```typescript
async function sendPayment(paymentRequest: string): SendPaymentResponse
```

### Response

```typescript
interface SendPaymentResponse {
  preimage: string;
}
```

### Example

```typescript
const invoice = "xxxx";
await window.$onekey?.webln.enable();
const info = await window.$onekey?.webln.sendPayment(invoice);
```

\##Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/OJdomvO" %}
