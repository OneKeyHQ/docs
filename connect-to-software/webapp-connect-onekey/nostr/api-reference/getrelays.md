# getRelays

### Method

<pre><code><strong>async function getRelays(): { [url: string]: {read: boolean, write: boolean} } 
</strong></code></pre>

### Response

returns a basic map of relay urls to relay policies

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.nostr) || window.nostr;
<strong>
</strong><strong>const event = async provider.getRelays()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/vYbzmoJ" %}
