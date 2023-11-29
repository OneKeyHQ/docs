# pushPsbt

Push Transaction

### Method

```typescript
async function pushPsbt(
    psbtHex: string, 
): string
```

### Params

* `rawtx` — _required_ `string`  the hex string of psbt to push

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const psbtHex = "010203"
</strong><strong>const txid = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.pushPsbt(psbtHex);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/dyaqzgE" %}

[^1]: 
