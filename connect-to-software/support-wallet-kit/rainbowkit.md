# Rainbowkit

## Docs <a href="#install" id="install"></a>

{% embed url="https://www.rainbowkit.com/docs/introduction" %}

## Install <a href="#install" id="install"></a>

{% tabs %}
{% tab title="Yarn" %}
yarn add @rainbow-me/rainbowkit wagmi viem
{% endtab %}

{% tab title="Npm" %}
npm install @rainbow-me/rainbowkit wagmi viem
{% endtab %}
{% endtabs %}

## Config OneKey

Add the OneKey Wallet Id when Rainbowkit.

```javascript
// import OneKey Wallet
import { oneKeyWallet } from "@rainbow-me/rainbowkit/wallets";

const appName = "OneKey Demo";
const projectId = "12345";

const { wallets } = getDefaultWallets({ appName, projectId, chains });
const connectors = connectorsForWallets([
  ...wallets,
  {
    groupName: "Recommend",
    // Config OneKey Wallet
    wallets: [oneKeyWallet({ chains })]
  }
]);

const wagmiConfig = createConfig({
  connectors,
  publicClient
});
```



## Quickstart

{% embed url="https://codesandbox.io/embed/rainbowkit-demo-hg62ng?autoresize=1&fontsize=14&hidenavigation=1&theme=dark" fullWidth="true" %}
