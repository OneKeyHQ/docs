# nip04.decrypt

### Method

<pre><code><strong>async function nip04.decrypt(pubkey, message): string 
</strong></code></pre>

### Params

* `pubkey` — _required_ `string`  the publick key
* `message` — _required_ `string` a string to sign

### Response

returns ciphertext and iv as specified in nip-04

### Example

```
const decrypted = async window.$onekey?.nostr.nip04.decrypt(pubkey, "010203")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/rNPZwaa" %}
