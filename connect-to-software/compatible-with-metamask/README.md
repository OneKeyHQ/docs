# MetaMask Compatibility

OneKey exposes an [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) Ethereum provider. Many EVM dApps can reuse their existing `request({ method, params })` integration, but must select the intended wallet when multiple extensions are installed.

## Select a Provider

For a wallet chooser, use the [EIP-6963 discovery example](../webapp-connect-onekey/eth/provider-api.md). For a button specifically labeled OneKey, select its namespaced provider and show an installation option if it is unavailable:

```javascript
const provider = window.$onekey?.ethereum;
if (!provider) throw new Error('Install or enable OneKey to continue.');

// Call from a Connect button handler.
const accounts = await provider.request({ method: 'eth_requestAccounts' });
```

The shared `window.ethereum` object can belong to another extension. Keep the selected provider for all account, network, signing, and event calls; do not silently switch wallets after the user makes a choice.

## Identify OneKey

`isOneKey` is a boolean property, not a function:

```javascript
if (provider?.isOneKey === true) {
  console.log('OneKey provider selected');
}
```

Provider flags and discovery metadata are identification hints supplied by the extension, not a security boundary.

## Wallet UI Libraries

Use a [wallet UI library](../support-wallet-kit/) if your application needs a multi-wallet connection dialog. Configure the OneKey connector for the library version you use.

See the [provider detection demo](detectethereumprovider.md) for an additional example.
