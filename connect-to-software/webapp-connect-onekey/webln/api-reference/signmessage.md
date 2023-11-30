# signMessage

Request that the user signs an string message.&#x20;

### Method

```typescript
async function signMessage(message: string): SignMessageResponse
```

### Response

```typescript
interface SignMessageResponse {
  message: string;
  signature: string;
}
```

### Example

```typescript
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

const message = "Plain Message";
await provider.enable();
const signature = await provider.signMessage(message);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/mdvGmLm" %}
