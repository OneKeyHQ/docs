# verifyMessage

Request the client to verify if the signature is correct, with the results displayed on the client side.

### Method

```typescript
async function verifyMessage(signature: string, message: string): void
```

### Example

```typescript
const message = "Plain Message";
await window.webln.enable();
const signature = window.webln.signMessage(message)
await window.webln.verifyMessage(signature, message);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/eYxLvXK" %}
