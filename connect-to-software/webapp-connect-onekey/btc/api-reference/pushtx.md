# pushTx

Push Transaction

### Method

```typescript
async function pushTx(rawtx: string): string
```

### Params

* `rawtx` — _required_ `string`  rawtx to push

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const rawtx = "010203"
</strong><strong>const txid = async provider.pushTx(rawtx);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/VwgGzxO" %}
