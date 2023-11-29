# getRelays

### Method

<pre><code><strong>async function getRelays(): { [url: string]: {read: boolean, write: boolean} } 
</strong></code></pre>

### Response

returns a basic map of relay urls to relay policies

### Example

<pre><code><strong>const event = async window.$onekey?.nostr.getRelays()
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/vYbzmoJ" %}
