# sendInscription

Send Inscription

### Method

```typescript
async function sendInscription(
    toAddress: string, 
    inscriptionId: string, 
    options?: { feeRate: number }
): string
```

### Params

* `toAddress` — _required_ `string`  the address to send
* `inscriptionId` — _required_ `string` the id of Inscription
* `options` — _optional_&#x20;
  * `feeRate`  — _required_ `number` the network fee rate

### Response

`txid` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const address = "010203"
</strong>const inscription = "010203"
<strong>const txid = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.sendInscription(address, inscription);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/MWLqvVE" %}

[^1]: 
