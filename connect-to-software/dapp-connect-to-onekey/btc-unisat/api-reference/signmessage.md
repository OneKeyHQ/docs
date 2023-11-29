# signMessage

Sign message

### Method

```typescript
async function signMessage(
    message: string, 
    type: string,
): string
```

### Params

* `message` — _required_ `string`  a string to sign
* `type` — _optional_ `string`  "ecdsa" | "bip322-simple". default is "ecdsa"

### Response

`signature` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const message = "010203"
</strong><strong>const signature = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.signMessage(message);
</strong></code></pre>



[^1]: 
