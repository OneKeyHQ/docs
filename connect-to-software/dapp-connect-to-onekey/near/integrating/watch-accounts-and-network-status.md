# Watch Accounts & Network Status

The OneKey provider emit some events, which you can watch the connected accounts or network changed from, and update states to your web applications.

### Add Event Listerners

```javascript
// accountsChanged
const onAccountsChanged = (payload) => {
  const accountId = payload?.accounts?.[0]?.accountId || '';
  console.log('onAccountsChanged', accountId);
}
provider.on('accountsChanged', onAccountsChanged);


// networkChanged
const onNetworkChanged = (payload) => {
  console.log('onNetworkChanged', payload.networkId);
}
provider.on('networkChanged', onNetworkChanged);
```

### Remove Event Listerners

Also, don't forget to remove listeners once you are done listening to them (for example on component unmount in React):

```javascript
provider.off('accountsChanged', onAccountsChanged);
provider.off('networkChanged', onNetworkChanged);
```
