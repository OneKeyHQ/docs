# Event

The OneKey Browser Extension provider implements the [Node.js `EventEmitter`](https://nodejs.org/api/events.html) API.

This sections details the events emitted via that API.

There are innumerable `EventEmitter` guides elsewhere, but you can listen for events like this:



## accountsChanged

When switching accounts.

```
window.$onekey.webln.on('accountsChanged', (accounts) => {  
    // Handle the new accounts, or lack thereof.
    // "accounts" will always be an array, but it can be empty.
});
window.$onekey.webln.off('accountsChanged');
```

## chainChanged **(Deprecated)**

When the chain changes.

```
window.$onekey.webln.on('chainChanged', (chainId) => {  
    // Handle the new chain.  
    // Correctly handling chain changes can be complicated.  
    // We recommend reloading the page unless you have good reason not to.  window.location.reload();
});
window.$onekey.webln.off('chainChanged');
```
