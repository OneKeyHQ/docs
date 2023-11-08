# Accessing Accounts

## Accessing Accounts

User accounts are used in a variety of contexts in Ethereum, including as identifiers and for signing transactions. In order to request a signature from the user or have the user approve a transaction, one must be able to access the user's accounts. The `wallet methods` below involve a signature or transaction approval and all require the sending account as a function parameter.

* `eth_sendTransaction`
* `eth_sign` (insecure and unadvised to use)
* `eth_personalSign`
* `eth_signTypedData`



**Connect Account Example:**

```javascript
async function getAccount() {
  const provider = window.$onekey.ethereum || window.ethereum;
  const accounts = await provider.request({ method: 'eth_requestAccounts' })
    .catch((err) => {
      if (err.code === 4001) {
        // EIP-1193 userRejectedRequest error
        // If this happens, the user rejected the connection request.
        console.log('Please connect to OneKey.');
      } else {
        console.error(err);
      }
    });
  const account = accounts[0];
}
```

If you'd like to be notified when the address changes, we have an event you can subscribe to:

```javascript
provider.on('accountsChanged', function (accounts) {  
    // Time to reload your interface with accounts[0]!
  if (accounts.length === 0) {
    // OneKey is locked or the user has not connected any accounts.
    console.log('Please connect to OneKey.');
  } else if (accounts[0] !== currentAccount) {
    // Reload your interface with accounts[0].
    currentAccount = accounts[0];
  }
});
```

If the first account in the returned array isn't the account you expected, you should notify the user! In the future, the accounts array may contain more than one account. However, the first account in the array will continue to be considered as the user's "selected" account.
