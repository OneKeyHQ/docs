# requestAccounts

To begin interaction, you first need to obtain the appropriate permissions. Calling requestAccounts() will prompt the user to allow the use of the browser's web access feature. After this, you are free to call any other API methods.

### Method

```typescript
async function requestAccounts(): string[]
```

### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>const accounts = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.requestAccounts()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/mdvGwga" %}

[^1]: 
