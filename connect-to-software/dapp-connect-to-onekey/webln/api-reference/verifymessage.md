# verifyMessage

Request the client to verify if the signature is correct, with the results displayed on the client side.

### Method

```typescript
async function verifyMessage(signature: string, message: string): void
```

### Example

```typescript
const message = "xxxx";
const signature = "xxxx";
await window.$onekey?.webln.enable();
await window.$onekey?.webln.verifyMessage(signature, message);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/eYxLvXK" %}
