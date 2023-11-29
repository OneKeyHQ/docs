# sendBitcoin

Send BTC

### Method

```typescript
async function sendBitcoin(
    toAddress: string, 
    satoshis: number, 
    options?: { feeRate: number }
): string
```

### Params

* `toAddress` — _required_ `string`  the address to send
* `satoshis` — _required_ `number` the satoshis to send
* `options` — _optional_&#x20;
  * `feeRate`  — _required_ `number` the network fee rate

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const address = "010203"
</strong><strong>const txid = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.sendBitcoin(address, 1000);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/RwvYZjX" %}

[^1]: 
