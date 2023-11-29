# signSchnorr

### Method

```typescript
async function signSchnorr(sigHash: string) : string;
```

### Response

Signature of arbitrary text signed with the private key of the active nostr account

### Example

```typescript
const signature = async window.$onekey?.nostr.signSchnorr("010203")
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/poGOPXB" %}
