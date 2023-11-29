# getInfo

Obtain node information and which methods are supported.

### Method

```typescript
async function getInfo(): GetInfoResponse
```

### Response

```typescript
export interface GetInfoResponse {
    node: {
        alias: string;
        pubkey: string;
        color?: string;
    };
    methods: string[];
}
```

### Example

```typescript
await window.$onekey?.webln.enable();
const info = await window.$onekey?.webln.getInfo();
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/QWYVvya" %}
