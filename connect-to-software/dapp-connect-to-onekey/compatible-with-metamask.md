# Compatible with Metamask

## Foreword

When it comes to DApps, one cannot help but mention Ethereum and EVM-compatible chains, which are almost the first choice for developers. MetaMask wallet is an inescapable Wallet. When developers create EVM DApps, the first they adapt to is MetaMask.&#x20;

OneKey, MetaMask, and a host of other wallets supporting EVM are all implemented based on the [EIP1193](https://eips.ethereum.org/EIPS/eip-1193) standard Provider API. Thus, under normal circumstances, there is no need to take any action; OneKey is fully compatible with MetaMask by default.&#x20;

However, there is a scenario where if you have both the OneKey and MetaMask extensions installed, when you use a DApp to connect, it may cause uncertainty as to which extension pops up to handle your request. It could be MetaMask or OneKey, which necessitates users to turn one off in order to use the other. Let’s take a look at our solution.



## Compatible with MetaMask

OneKey provides two Provider APIs: `window.ethereum` and `window.$onekey.ethereum`.

```javascript
const provider = window.$onekey.ethereum || window.ethereum
// Connect to Wallet
await provider.enable()
```

You can use the method above to obtain the provider, giving priority to the Provider API from `window.$onekey.ethereum`. If the OneKey extension is not installed, you can continue to use the Provider API provided by other wallets. However, if the OneKey extension is installed, but window.ethereum is still OneKey's Provider API, the aforementioned problem still exists.

This brings us to a feature of OneKey, the Wallet Switch, which can disable the MetaMask compatibility, ensuring that OneKey will only inject the window.$onekey.ethereum Provider API to enhance user experience. Of course, if you find OneKey very useful and do not want to use other software, it is still recommended to keep the Wallet Switch on.

| ![](<../../.gitbook/assets/image (1).png>) | ![](../../.gitbook/assets/image.png) |
| ------------------------------------------ | ------------------------------------ |



## How to Determine if it is OneKey Provider

OneKey's injected Provider API has a distinct API. Calling isOneKey will return true, indicating that the current Provider API is provided by OneKey.

```
provider.isOneKey
```
