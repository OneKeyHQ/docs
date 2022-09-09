# Detecting Provider Network

You can detect the provider network seleted in OneKey Wallet, currently supports both  `mainnet` and `testnet`.

```javascript
const res = await provider.request({
  method: 'near_networkInfo',
});
console.log('near_networkInfo', res, res?.networkId);

```
