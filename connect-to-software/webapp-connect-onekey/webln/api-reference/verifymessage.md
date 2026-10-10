# verifyMessage

Request the client to verify if the signature is correct, with the results displayed on the client side.

## Method

```typescript
declare function verifyMessage(signature: string, message: string): Promise<void>;
```

### Example

```typescript
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

const message = "Plain Message";
await provider.enable();
const { signature } = await provider.signMessage(message);
await provider.verifyMessage(signature, message);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/eYxLvXK" %}

The promise resolves without a value. This method does not return a boolean; handle a rejected promise as a failed verification request.
