# inscribeTransfer (Not Support)

Inscribe Transfer

### Method

```typescript
async function inscribeTransfer(
    ticker: string, 
    amount: string,
): string
```

### Params

* `ticker` — _required_ `string`  the hex string of psbt to sign
* `amount` — _required_ `string`&#x20;

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const ticker = "010203"
</strong>const amount = "100"
<strong>const txid = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.signPsbt(psbtHex);
</strong></code></pre>



[^1]: 
