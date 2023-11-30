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

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const address = "010203"
</strong><strong>const txid = async provider.sendBitcoin(address, 1000);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/RwvYZjX" %}
