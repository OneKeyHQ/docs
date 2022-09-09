# Signing Messages

When the web application is connected to OneKey, it can also request that the user signs a given message.&#x20;

In order to send a message for the user to sign, the web application must:&#x20;

* Provide UTF-8 encoded string.
* Have it be signed by the user's OneKey Wallet.

The method call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `signatures`.

```javascript
const res = await provider.request({
  method: 'near_signMessages',
  params: {
    messages: ['hello world', 'onekey near wallet'], // must be Array type
  },
});
console.log('near_signMessages', res, res.signatures);
```
