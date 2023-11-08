# DApp Developer

## Download OneKey

To develop for OneKey Browser Extension or Mobile Apps, Please install it on your development machine. [Download here.](https://onekey.so/download?client=browserExtension)

{% hint style="info" %}
This guide assumes intermediate knowledge of HTML, CSS, and JavaScript.
{% endhint %}

Once OneKey is installed and running, you should find that new browser tabs have a `window.$onekey` object available in the developer console. This is how your website will interact with OneKey.

## Intro

As a DApp developer, you may choose the following paths:

## OneKey Provider API

OneKey supports cross-chain DApp access, ensuring full compatibility across different platforms. For details.

| Client                              | DApp support                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------- |
| OneKey Chrome plugin                | You can use DApp to connect to OneKey in Chrome.                                |
| OneKey Edge plugin                  | You can use DApp to connect to OneKey in Edge.                                  |
| OneKey Desktop（Windows、macOS、Linux） | You can use DApp to connect to OneKey in Built-in Browser on the Desktop.       |
| OneKey Mobile client（iOS、Android）   | You can use DApp to connect to OneKey in Built-in Browser on the Mobile client. |

[Read more >>>](../connect-to-software/dapp-connect-to-onekey/)

## Use **Wallet Kit SDK & Wallet Aggregator SDKs**

Use a wallet aggregator SDK that supports OneKey.&#x20;

> If you already have access to the following SDK can also refer to the related links.

* Using Metamask SDK or Compatible OneKey [Read more >>>](../connect-to-software/compatible-with-metamask/)
* Using Web3 Onboard or Compatible OneKey [Read more >>>](../connect-to-software/compatible-with-metamask/third-party-wallet-kit/web3-onboard.md)
* Using Rainbowkit or Compatible OneKey [Read more >>>](../connect-to-software/compatible-with-metamask/third-party-wallet-kit/rainbowkit.md)

## WalletConnect Integration

We support two WalletConnect methods to connect to OneKey:

* DApp developers can connect to OneKey via the「WalletConnect QR code」.

### Support for Scanning WalletConnect QR Code to Connect Your Application

* If you are using a DApp application, you can choose the WalletConnect connection method. Use the OneKey software wallet to scan the WalletConnect QR code to instantly connect to the DApp. You can then use the assets managed in OneKey to use the corresponding DApp.
* DApp developers who want to add WalletConnect connectivity to their projects to connect to OneKey can [refer to the relevant WalletConnect documentation](https://docs.walletconnect.com/quickstart).

