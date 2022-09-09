# Signing Transaction

Signing transactions without sending.&#x20;

The method call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `signatures`.

```javascript
const transaction = ... // create Transaction first

const res = await provider.request({
  method: 'near_signTransactions',
  params: {
    transactions: [transaction], // must be Array type
  },
});
console.log('near_signTransactions', res, res.signatures);
```
