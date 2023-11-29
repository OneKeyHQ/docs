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
const message = "Plain Message";
await window.webln.enable();
const signature = await window.webln.signMessage(message);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/mdvGmLm" %}
