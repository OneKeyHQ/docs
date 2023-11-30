# verifyMessage

Request the client to verify if the signature is correct, with the results displayed on the client side.

### Method

```typescript
async function verifyMessage(signature: string, message: string): void
```

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.webln) || window.webln;

const message = "Plain Message";
await provider.enable();
<strong>const signature = await provider.signMessage(message)
</strong>await provider.verifyMessage(signature, message);
</code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/eYxLvXK" %}
