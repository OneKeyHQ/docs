# Detect an Ethereum Provider

Start with the [provider detection guide](../webapp-connect-onekey/eth/provider-api.md) for EIP-6963 discovery and direct OneKey selection. Detecting a provider does not authorize account access; call `eth_requestAccounts` from a user action after selection.

## Demo

The embedded demo is an additional integration reference. Check its dependency versions before copying it into a current application.

{% embed url="https://codesandbox.io/embed/detect-ethereum-provider-q8chv5?autoresize=1&expanddevtools=1&fontsize=14&hidenavigation=1&module=%2Fsrc%2FEthereumProvider%2Findex.ts&theme=dark" fullWidth="true" %}
