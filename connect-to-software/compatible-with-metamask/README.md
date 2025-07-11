# Compatible with Metamask

## **Introduction**

In the development of blockchain applications, Ethereum and its compatible EVM chains are the first choice for developers. MetaMask wallet, as a key component for EVM-based DApp development, is often the primary object for compatibility testing.&#x20;

Wallets developed for EVM chains, such as OneKey and MetaMask, both follow the [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) standard to implement the Provider API. In most cases, a DApp that is compatible with MetaMask can be used directly with OneKey without any modification.&#x20;

In special cases, when both OneKey and MetaMask extensions are present, a conflict may occur during the DApp connection process. This conflict results in an inability to select which extension should respond to the connection request, and users might need to disable one extension to continue.&#x20;

To address this issue, we provide the following solutions: Users can manually disable an extension to prevent conflicts. Developers can specify in the code which Provider API to use preferentially, reducing the steps users need to take. With these solutions, both users and developers can enjoy a smoother experience, whether they are using OneKey or MetaMask wallets.

## Compatible with MetaMask

OneKey provides the Provider API through window.ethereum and window.$onekey.ethereum. Developers should call `window.$onekey.ethereum` preferentially, and if OneKey is not installed, then use other wallets' API.

<pre class="language-javascript"><code class="lang-javascript">const provider = (window.$onekey &#x26;&#x26; window.$onekey.ethereum) || window.ethereum;
// Connect to Wallet
<strong>await provider.enable()
</strong></code></pre>

[Full Demo >>>](detectethereumprovider.md)

If OneKey is installed, `window.ethereum` may point to OneKey API, which could lead to a plugin conflict. To address this, OneKey has introduced the Wallet Switch feature, which turns off MetaMask compatibility mode to avoid conflicts and enhance the experience.

To resolve this issue, OneKey has launched the Wallet Switch feature. This feature, by turning off MetaMask's compatibility mode, ensures that OneKey only injects its exclusive `window.$onekey.ethereum` Provider API, avoiding potential conflicts and thus optimizing the user experience. If users prefer OneKey and want to avoid frequent wallet switching, it is recommended to enable the Wallet Switch feature.

<table data-header-hidden><thead><tr><th width="349"></th><th></th></tr></thead><tbody><tr><td><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""></td><td><img src="../../.gitbook/assets/image (8) (1).png" alt=""></td></tr></tbody></table>

## Using Wallet Kit

There are also some wallet Kits that support the OneKey API that can be used directly.

* [Web3 Onboard >>>](../support-wallet-kit/web3-onboard.md)
* [Rainbow Kit >>>](../support-wallet-kit/rainbowkit.md)

> With wallet kit, you don't have to worry about and consider the above adaptation problems with Metamask, which is handled in the kits.

## How to Determine if it is OneKey Provider

OneKey's Provider API offers a unique method, `isOneKey`. Developers can call this method, and if the return value is `true`, then it is confirmed that the current Provider API is injected by OneKey. This helps to accurately identify and select the correct Provider in a multi-wallet scenario, ensuring the DApp operates normally.

An example of use is as follows:

```
if (provider.isOneKey && provider.isOneKey()) {
  console.log('The current provider comes from OneKey');
}
```

This method helps to avoid conflicts in environments where both OneKey and other wallets that follow the EIP-1193 standard (like MetaMask) are installed.
