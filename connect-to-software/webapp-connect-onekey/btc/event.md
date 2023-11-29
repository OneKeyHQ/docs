# Event

The OneKey Browser Extension provider implements the [Node.js `EventEmitter`](https://nodejs.org/api/events.html) API.

This sections details the events emitted via that API.

There are innumerable `EventEmitter` guides elsewhere, but you can listen for events like this:



## accountsChanged

When switching accounts.

```
window.$onekey.btc.on('accountsChanged', (accounts) => {  
    // Handle the new accounts, or lack thereof.
    // "accounts" will always be an array, but it can be empty.
});
window.$onekey.btc.off('accountsChanged');
```

## networkChanged

When the chain changes.

```
window.$onekey.btc.on('networkChanged', (chainId) => {  
    // Handle the new chain.  
    // Correctly handling chain changes can be complicated.  
    // We recommend reloading the page unless you have good reason not to.  window.location.reload();
});
window.$onekey.btc.off('chainChanged');
```
