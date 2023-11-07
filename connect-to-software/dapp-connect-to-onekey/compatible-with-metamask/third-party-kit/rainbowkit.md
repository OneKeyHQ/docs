# Rainbowkit

## Install <a href="#install" id="install"></a>

{% tabs %}
{% tab title="Yarn" %}
yarn add @rainbow-me/rainbowkit wagmi viem
{% endtab %}

{% tab title="Npm" %}
npm install @rainbow-me/rainbowkit wagmi viem
{% endtab %}
{% endtabs %}

## Quickstart

```javascript
import {
  getDefaultWallets,
  connectorsForWallets,
} from '@rainbow-me/rainbowkit';
import { oneKeyWallet } from '@rainbow-me/rainbowkit/wallets';
import { createConfig, WagmiConfig, configureChains, mainnet } from 'wagmi';
import { alchemyProvider } from 'wagmi/providers/alchemy';
import { publicProvider } from 'wagmi/providers/public';

const { chains, publicClient } = configureChains(
  [mainnet],
  [
    alchemyProvider({ apiKey: process.env.ALCHEMY_ID }),
    publicProvider(),
  ]
);

const { wallets } = getDefaultWallets({ appName, projectId, chains });
const connectors = connectorsForWallets([
  ...wallets,
  {
    groupName: 'Other',
    wallets: [oneKeyWallet({ chains })],
  },
]);

const wagmiConfig = createConfig({
  connectors,
  publicClient
});

const App = () => (
  <WagmiConfig config={wagmiConfig}>
    <RainbowKitProvider {...etc}>
      {/* Your App */}
    </RainbowKitProvider>
  </WagmiConfig>
);
```
