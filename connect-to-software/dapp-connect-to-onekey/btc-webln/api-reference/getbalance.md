# getBalance

Check account balance

### Method

```typescript
async function getBalance(): BalanceResponse
```

### Response

```typescript
interface BalanceResponse {
    balance: number;
    currency?: "sats" | "EUR" | "USD"
}
```

### Example

```typescript
await window.$onekey?.webln.enable();
const balance = await window.$onekey?.webln.getBalance();
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/GRzXmqN" %}
