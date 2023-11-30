# pushPsbt

Push Transaction

### Method

```typescript
async function pushPsbt(psbtHex: string): string
```

### Params

* `rawtx` — _required_ `string`  the hex string of psbt to push

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const psbtHex = "010203"
</strong><strong>const txid = async provider.pushPsbt(psbtHex);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/dyaqzgE" %}
