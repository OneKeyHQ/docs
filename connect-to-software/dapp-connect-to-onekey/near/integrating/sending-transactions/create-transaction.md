# Create Transaction

You should create transaction object by [near-api-js](https://docs.near.org/docs/tutorials/create-transactions#constructing-the-transaction), and we provide some convenience methods to get nonce and lasted block hash that is required by transaction constructor.

```javascript
import * as nearAPI from "near-api-js";

// must equal to provider connected account
const sender = 'c3be856133196da252d0f1083614cdc87a85c8aa8abeaf87daff1520355eec53'; 
// must equal to provider connected account
const publicKey = 'ed25519:EB6zw1gAr4hV6NHuk9rhFPR8m8MwJbyqvZaweXiTNZ2J'; 
const receiver = 'c3be856133196da252d0f1083614cdc87a85c8aa8abeaf87daff1520355eec53';

const amount = nearAPI.utils.format.parseNearAmount('0.0001');
const actions = [nearAPI.transactions.transfer(amount)];

const accessKey = await provider.request({
  method: 'query',
  params: [`access_key/${sender}/${publicKey}`, '']
});
// nonce increase 1
const nonce = accessKey.nonce + 1;
const recentBlockHash = nearAPI.utils.serialize.base_decode(
  accessKey.block_hash
);

const publicKeyBytes = nearAPI.utils.PublicKey.fromString(publicKey);

const transaction = nearAPI.transactions.createTransaction(
  sender,
  publicKeyBytes,
  receiver,
  nonce,
  actions,
  recentBlockHash,
);
```

Or just call `provider.createTransaction` method more easier.

```javascript
const receiver = 'c3be856133196da252d0f1083614cdc87a85c8aa8abeaf87daff1520355eec53';
const amount = nearAPI.utils.format.parseNearAmount('0.0001');
const actions = [nearAPI.transactions.transfer(amount)];

const transaction = await provider.createTransaction({
  receiverId: receiver,
  actions,
  // nonce increase 1
  nonceOffset: 1,
});
```
