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

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const balance = async provider.getBalance();
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/qBgMXXJ" %}
