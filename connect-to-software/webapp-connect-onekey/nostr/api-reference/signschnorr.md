# signSchnorr

### Method

```typescript
async function signSchnorr(sigHash: string) : string;
```

### Params

* `sigHash` — _required_ `string`  a hash to sign

### Response

Signature of arbitrary text signed with the private key of the active nostr account

### Example

```typescript
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

const signature = async provider.signSchnorr("010203")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/poGOPXB" %}
