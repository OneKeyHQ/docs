# getBalance

Get balance of current account.

### Method

```typescript
async function getBalance(): BalanceInfo
```

### Response

the balance.

```typescript
type BalanceInfo = { 
    confirmed: number; 
    unconfirmed: number; 
    total: number 
};
```

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const balance = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.getBalance();
</strong></code></pre>

[^1]: 
