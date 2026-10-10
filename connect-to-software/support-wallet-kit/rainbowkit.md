# RainbowKit

Add the built-in OneKey wallet to a custom wallet list. The following configuration uses the RainbowKit 2 / Wagmi 2 API. Follow the [RainbowKit installation guide](https://www.rainbowkit.com/docs/installation) for the required React providers and styles.

## Install

```bash
npm install @rainbow-me/rainbowkit@^2 wagmi@^2 viem@^2 @tanstack/react-query
```

## Configure OneKey

```typescript
import { connectorsForWallets } from '@rainbow-me/rainbowkit';
import { oneKeyWallet } from '@rainbow-me/rainbowkit/wallets';
import { createConfig, http } from 'wagmi';
import { mainnet } from 'wagmi/chains';

const connectors = connectorsForWallets(
  [{ groupName: 'Recommended', wallets: [oneKeyWallet] }],
  { appName: 'My dApp', projectId: '<WALLETCONNECT_PROJECT_ID>' },
);

export const config = createConfig({
  chains: [mainnet],
  connectors,
  transports: { [mainnet.id]: http() },
});
```

Replace the project ID and configure the networks and RPC endpoints your application supports. See the [custom wallet list reference](https://www.rainbowkit.com/docs/custom-wallet-list) for adding other wallets. Do not mix this configuration with Wagmi 1 `publicClient` examples.

## Earlier Demo

This demo may use older dependency versions; check its package configuration before reuse.

{% embed url="https://codesandbox.io/embed/rainbowkit-demo-hg62ng?autoresize=1&fontsize=14&hidenavigation=1&theme=dark" fullWidth="true" %}
