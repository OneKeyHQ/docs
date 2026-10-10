# Web3Modal (Legacy)

## Documentation

{% embed url="https://docs.walletconnect.com/web3modal/about" %}

This page preserves a legacy React integration using Wagmi 1 and viem 1. For new integrations, consult [Reown AppKit](https://docs.reown.com/appkit/overview), the successor to Web3Modal. Do not combine this example with current Wagmi configuration without following the migration guide.

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

## Configure OneKey

Add the OneKey wallet ID when calling `createWeb3Modal`. Initialize `wagmiConfig`, `projectId`, and `chains` using the matching legacy Web3Modal setup first.

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
