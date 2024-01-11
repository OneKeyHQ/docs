# Web3Modal

## Doc

{% embed url="https://docs.walletconnect.com/web3modal/about" %}

Here is the react demo, please refer to the official website for more information.



## Install

{% tabs %}
{% tab title="Npm" %}
```
npm install @web3modal/wagmi wagmi@1.4.13 viem@1.21.4
```
{% endtab %}

{% tab title="Yarn" %}
```
yarn add @web3modal/wagmi wagmi@1.4.13 viem@1.21.4
```
{% endtab %}
{% endtabs %}



## Config OneKey

Add the OneKey Wallet Id when createWeb3Modal.

```
createWeb3Modal({
  wagmiConfig,
  projectId,
  chains,
  featuredWalletIds: [
    // OneKey Wallet Id more see https://explorer.walletconnect.com/onekey
    "1aedbcfc1f31aade56ca34c38b0a1607b41cccfa3de93c946ef3b4ba2dfab11c",
  ],
  includeWalletIds: [
    // OneKey Wallet Id more see https://explorer.walletconnect.com/onekey
    "1aedbcfc1f31aade56ca34c38b0a1607b41cccfa3de93c946ef3b4ba2dfab11c",
  ],
});
```

## Quickstart & Demo

{% embed url="https://codesandbox.io/embed/xd9wjd?module=/src/App.tsx&view=Editor+++Preview" fullWidth="true" %}
