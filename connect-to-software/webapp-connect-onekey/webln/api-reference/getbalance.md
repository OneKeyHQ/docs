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
await window.webln.enable();
const balance = await window.webln.getBalance();
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/GRzXmqN" %}
