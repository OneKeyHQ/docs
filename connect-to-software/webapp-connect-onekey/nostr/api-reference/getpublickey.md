# getPublicKey

### Method

```typescript
async function getPublicKey(): string 
```

### Response

returns a public key as hex

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const provider = (window.$onekey &#x26;&#x26; window.$onekey.nostr) || window.nostr;
</strong><strong>
</strong><strong>const pubkey = async provider.getPublicKey()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/ExremGa" %}
