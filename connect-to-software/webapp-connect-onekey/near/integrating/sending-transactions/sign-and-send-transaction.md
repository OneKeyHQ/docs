# Sign and Send Transaction

Once a transaction is created, the web application may ask the user's OneKey wallet to sign and send the transaction using their account's private key and NEAR JSON RPC connection.&#x20;

By far the **easiest** and **recommended** way of doing this is by using the `requestSignTransactions` method on the provider, but it is also possible to do with `request`.&#x20;

In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `transactionHashes`.

{% tabs %}
{% tab title="requestSignTransactions()" %}
```javascript
const transaction = ... // create Transaction first

const res = await provider.requestSignTransactions({
  transactions: [transaction], // must be Array type
});
console.log('requestSignTransactions', res, res.transactionHashes);

```
{% endtab %}

{% tab title="request()" %}
```javascript
const transaction = ... // create Transaction first

const res = await provider.request({
  method: 'near_sendTransactions',
  params: {
    transactions: [transaction], // must be Array type
  },
});
console.log('near_sendTransactions', res, res.transactionHashes);
```
{% endtab %}
{% endtabs %}

Also transaction can be serialized to **`base64`** string first before method calling.

```javascript
const transaction = ... // create Transaction first

const transactionSerialized = transaction.encode().toString('base64');

const res = await provider.request({
  method: 'near_sendTransactions',
  params: {
    transactions: [transactionSerialized], // must be Array type
  },
});
console.log('near_sendTransactions', res, res.transactionHashes);
```
