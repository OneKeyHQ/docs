# Accessing Accounts

After wallet connection established, you can access the accounts connected with the OneKey wallet.&#x20;

User accounts are used in a variety of contexts in OneKey, including as identifiers and for signing transactions.

```javascript
const res = await provider.request({
  method: 'near_accounts',
});
const accountId = res?.accounts?.[0]?.accountId || '';
const publicKey = res?.accounts?.[0]?.publicKey || '';

// if use reject wallet connection, res?.accounts should be []
console.log('near_accounts', res?.accounts, accountId, publicKey);
```
