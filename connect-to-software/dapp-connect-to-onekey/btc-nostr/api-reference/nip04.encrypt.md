# nip04.encrypt

### Method

<pre><code><strong>async function nip04.encrypt(pubkey, plaintext): string 
</strong></code></pre>

### Response

returns ciphertext and iv as specified in nip-04

### Params

```typescript
pubkey: string
plaintext: string
```

### Example

```
const encrypted = async window.$onekey?.nostr.nip04.encrypt(pubkey, "010203")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/rNPZwaa" %}
