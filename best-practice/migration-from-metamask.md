# Migrate from MetaMask

Reuse your EIP-1193 request flow and select OneKey explicitly. Use [EIP-6963 discovery](../connect-to-software/webapp-connect-onekey/eth/provider-api.md) for a multi-wallet chooser, or `window.$onekey.ethereum` for a OneKey-specific connection button.

```javascript
const provider = window.$onekey?.ethereum;
if (!provider) throw new Error('OneKey Ethereum provider is unavailable.');

try {
  const accounts = await provider.request({ method: 'eth_requestAccounts' });
  console.log(accounts);
} catch (error) {
  console.error(error.code, error.message);
}
```

## Migration Checklist

* Keep one selected provider for account requests, signatures, transactions, and event listeners.
* Replace deprecated `enable()` calls with `request({ method: 'eth_requestAccounts' })`.
* Treat `isOneKey` as a boolean property. `isMetaMask` alone does not uniquely identify MetaMask in a compatibility environment.
* Handle `accountsChanged`, `chainChanged`, and rejected promises.
* Verify wallet-specific methods in the target wallet version instead of assuming that every MetaMask extension method is supported.

## Legacy Provider Switching

Older OneKey integrations used `window.onekey` and `window.ethereum.switchProvider()`. `switchProvider` is not part of EIP-1193 and is not a reliable cross-wallet selection mechanism. Prefer discovery or the namespaced provider for new code. If maintaining an older integration, feature-detect the legacy method and verify its behavior against that extension version before calling it.
