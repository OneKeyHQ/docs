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

```
const encrypted = async window.$onekey?.nostr.nip04.encrypt(pubkey, "010203")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/rNPZwaa" %}
