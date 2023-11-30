# getPublicKey

Get publicKey of current account.

### Method

```typescript
async function getPublicKey(): string
```

### Params

the publicKey.

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const publicKey = async provider.getPublicKey();
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/jOdvLLy" %}
