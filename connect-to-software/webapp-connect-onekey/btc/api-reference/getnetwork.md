# getNetwork

Get network of current account

### Method

```typescript
async function getNetwork(): string
```

### Response

the network. `livenet` and `testnet`

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const network = async provider.getNetwork()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/xxMaLqz" %}
