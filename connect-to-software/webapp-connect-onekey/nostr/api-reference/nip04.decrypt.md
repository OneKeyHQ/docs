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

```javascript
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

const message = "Data to be encrypted";
const encrypted = await provider.nip04.encrypt(pubkey, message);
const decrypted = await provider.nip04.decrypt(pubkey, encrypted); // Data to be encrypted
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/rNPZwaa" %}
