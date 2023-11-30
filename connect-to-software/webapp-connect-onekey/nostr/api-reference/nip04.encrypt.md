# nip04.encrypt

### Method

<pre><code><strong>async function nip04.encrypt(pubkey, plaintext): string 
</strong></code></pre>

### Response

returns ciphertext and iv as specified in nip-04

### Params

* `pubkey` — _required_ `string`  the publick key
* `message` — _required_ `string` a string to sign

### Example

```javascript
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

const encrypted = async provider.nip04.encrypt(pubkey, "Data to be encrypted")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/rNPZwaa" %}
