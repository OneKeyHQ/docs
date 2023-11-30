# enable

To begin interaction, you first need to obtain the appropriate permissions. Calling webn.enable() will prompt the user to allow the use of the browser's web access feature. After this, you are free to call any other API methods.

### Method

```typescript
async function enable(): void
```



### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.webln) || window.webln;

<strong>async provider.enable()
</strong></code></pre>



### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/dyaqWoy" %}
