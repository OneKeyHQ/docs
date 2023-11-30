# getInscriptions

List inscriptions of current account.

### Method

```typescript
async function getInscriptions(cursor: number, size: number): InscriptionResponse
```

### Params

* `cursor` — _optional_ `number`  Initial index, Default is `0`
* `size` — _optional_ `number` Query quantity, Default is `20`

### Response

```typescript
export type InscriptionResponse = {
  total: number;
  list: InscriptionInfo[];
};

export type InscriptionInfo = {
  inscriptionId: string;
  inscriptionNumber: number;
  address: string;
  outputValue: number;
  preview: string;
  content: string;
  contentLength: number;
  contentType: string;
  timestamp: number;
  genesisTransaction: string;
  location: string;
  output: string;
  offset: number;
};
```

### Example

<pre class="language-typescript"><code class="lang-typescript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.btc) || window.unisat;

<strong>const inscriptions = async provider.getInscriptions();
</strong></code></pre>

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/MWLqvEv" %}
