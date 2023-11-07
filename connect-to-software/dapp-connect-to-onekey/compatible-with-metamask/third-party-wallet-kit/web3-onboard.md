# Web3 Onboard

## Install <a href="#install" id="install"></a>

{% tabs %}
{% tab title="Yarn" %}
yarn add @web3-onboard/core @web3-onboard/injected-wallets
{% endtab %}

{% tab title="Npm" %}
npm install @web3-onboard/core @web3-onboard/injected-wallets
{% endtab %}
{% endtabs %}

## Quickstart

```javascript
import Onboard from '@web3-onboard/core'
import injectedModule, { ProviderLabel } from '@web3-onboard/injected-wallets'

const MAINNET_RPC_URL = 'https://mainnet.infura.io/v3/<INFURA_KEY>'

const injected = injectedModule({
  // Displays all unavailable wallets
  displayUnavailable: true,
  // Filter is not available for wallets, only partial display is allowed
  // The one key plug-in is also displayed if the browser does not have the one key plug-in installed
  displayUnavailable: [ProviderLabel.OneKey, ProviderLabel.Metamask]
})

const onboard = Onboard({
  wallets: [injected]
  chains: [
    {
      id: '0x1',
      token: 'ETH',
      label: 'Ethereum Mainnet',
      rpcUrl: MAINNET_RPC_URL
    }
  ]
  //... other options
})

const wallets = await onboard.connectWallet()
```
