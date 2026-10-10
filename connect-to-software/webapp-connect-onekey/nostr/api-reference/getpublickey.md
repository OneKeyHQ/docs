# getPublicKey

## Method

```typescript
declare function getPublicKey(): Promise<string>;
```

### Response

returns a public key as hex

### Example

```typescript
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

const pubkey = await provider.getPublicKey()
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/ExremGa" %}
