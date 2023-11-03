# Compatible with Metamask

## **Introduction**

In the field of blockchain application development, Ethereum and its compatible EVM chains are among the preferred platforms for developers. On these platforms, the MetaMask wallet plays a pivotal role, thus ensuring compatibility with MetaMask is typically a priority when adapting DApps for EVM.

For EVM-compatible wallets such as OneKey and MetaMask, both adhere to the [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) standard, implementing the Provider API. Generally, OneKey can seamlessly interface with the API provided by MetaMask without special configuration.

However, when both OneKey and MetaMask extensions are installed, there might be an issue determining which extension is responding to the wallet connection requests within a DApp. This necessitates the deactivation of one extension to continue, for which we offer the following solution.



## Compatible with MetaMask

OneKey provides two types of Provider API interfaces: `window.ethereum` and `window.$onekey.ethereum`.

```javascript
const provider = window.$onekey.ethereum || window.ethereum
// Connect to Wallet
await provider.enable()
```

The above code retrieves the Provider, with priority given to the API of `window.$onekey.ethereum`. If the OneKey extension is not installed, it will fallback to another wallet's API. If OneKey is installed but `window.ethereum` still points to OneKey's API, the extension conflict issue remains unresolved.

To address this, OneKey introduced the Wallet Switch feature, which disables compatibility with MetaMask, ensuring that only `window.$onekey.ethereum` is injected to enhance user experience. Of course, if users prefer OneKey and do not wish to switch to other wallets, it is recommended to enable the Wallet Switch feature.

| ![](<../../.gitbook/assets/image (1).png>) | ![](../../.gitbook/assets/image.png) |
| ------------------------------------------ | ------------------------------------ |



## **Identifying OneKey's Provider**

The Provider API injected by OneKey includes a unique API method `isOneKey`. If this method returns `true`, it indicates

```
if (provider.isOneKey && provider.isOneKey()) {
  console.log('The current provider comes from OneKey');
}
```
