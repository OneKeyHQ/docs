# signMessage

Sign message

### Method

```typescript
async function signMessage(message: string, type: string): string
```

### Params

* `message` — _required_ `string`  a string to sign
* `type` — _optional_ `string`  "ecdsa" | "bip322-simple". default is "ecdsa"

### Response

`signature` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const message = "010203"
</strong><strong>const signature = async provider.signMessage(message);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/OJdojZM" %}
