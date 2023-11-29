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

<pre class="language-typescript"><code class="lang-typescript"><strong>const inscriptions = async window.$onekey?.<a data-footnote-ref href="#user-content-fn-1">btc</a>.getInscriptions();
</strong></code></pre>

[^1]: 
