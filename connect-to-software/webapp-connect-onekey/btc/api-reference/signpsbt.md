# signPsbt

Sign PSBT

### Method

```typescript
async function signPsbt(
    psbtHex: string, 
    options: { autoFinalized: boolean },
): string
```

### Params

* `psbtHex` — _required_ `string`  the hex string of psbt to sign
* `options` — _optional_ `object`&#x20;
  * `autoFinalized` — _optional_ `boolean`: whether finalize psbt after signing, default is `true`

### Response

`signed` — `string`

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const psbtHex = "010203"
</strong><strong>const signed = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.signPsbt(psbtHex);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/GRzXvXJ" %}

[^1]: 
