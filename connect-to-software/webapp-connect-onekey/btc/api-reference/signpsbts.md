# signPsbts

Sign PSBT List

### Method

```typescript
async function signPsbts(
    psbtHex: string, 
    options: { autoFinalized: boolean },
): string[]
```

### Params

* `psbtHexs` — _required_ `Array<string>`  the hex string of psbt to sign
* `options` — _optional_ `object`&#x20;
  * `autoFinalized` — _optional_ `boolean`: whether finalize psbt after signing, default is `true`

### Response

`signeds` — `Array<string>`

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const psbtHexs = ["010203", "010203"]
</strong><strong>const signeds = async provider.signPsbts(psbtHexs);
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/XWOPaPY" %}
