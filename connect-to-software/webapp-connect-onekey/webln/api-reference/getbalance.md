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
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

await provider.enable();
const balance = await provider.getBalance();
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/GRzXmqN" %}
