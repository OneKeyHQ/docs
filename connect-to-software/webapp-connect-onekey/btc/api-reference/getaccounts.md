# getAccounts

Get address of current account

### Method

```typescript
async function getAccounts(): string[]
```

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const accounts = async provider.getAccounts()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/qBgMXRM" %}
