# getInfo

Obtain node information and which methods are supported.

### Method

```typescript
async function getInfo(): GetInfoResponse
```

### Response

```typescript
interface GetInfoResponse {
  node: {
    alias: string;
    pubkey: string;
    color?: string;
  };
  // "request.*" methods are not supported by all connectors
  // (see webln.request for more info)
  methods: string[]; // e.g. "makeInvoice", "sendPayment", "request.openchannel", ...
}
```

### Example

```typescript
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

await provider.enable();
const info = await provider.getInfo();
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/QWYVvya" %}
