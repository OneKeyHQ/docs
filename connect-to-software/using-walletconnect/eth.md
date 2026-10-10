# Ethereum and EVM with WalletConnect

For an EVM connection dialog, start with [RainbowKit](../support-wallet-kit/rainbowkit.md) or the [Web3Modal / AppKit guidance](../support-wallet-kit/web3modal.md). Configure a project ID, application metadata, and the chains your dApp supports.

After the user connects, use the selected connector's provider for account access, requests, and events. A WalletConnect session does not inject `window.$onekey.ethereum`; that namespace belongs to the browser provider integration.

Handle session rejection, disconnection, and account or chain changes in your application. Follow the [WalletConnect documentation](https://docs.walletconnect.network/) for the protocol and [Reown AppKit documentation](https://docs.reown.com/appkit/overview) for a connection UI.
