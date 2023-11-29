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

<pre class="language-typescript"><code class="lang-typescript"><strong>const rawtx = "010203"
</strong><strong>const txid = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.pushTx(rawtx);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/VwgGzxO" %}

[^1]: 
