# Getting Started

## Download OneKey

To develop for OneKey Browser Extension or Mobile Apps, Please install it on your development machine. [Download here.](https://onekey.so/download?client=browserExtension)

{% hint style="info" %}
This guide assumes intermediate knowledge of HTML, CSS, and JavaScript.
{% endhint %}

Once OneKey is installed and running, you should find that new browser tabs have a `window.$onekey` object available in the developer console. This is how your website will interact with OneKey.

## Getting Started

OneKey aims to provide a convenient way to help you connect and manage digital assets. Whether you are a DApp developer, a third-party wallet developer, or a blockchain technician, OneKey offers easy access methods. This guide will lead you to quickly understand how to access OneKey, utilize its functions, and develop applications.

## DApp Developer Guide

As a DApp developer, you may choose the following paths:

1. Connect to OneKey through the OneKey Provider API. [Please read the API Access Guide for details.](../connect-to-software/dapp-connect-to-onekey/)
2. Connect to the OneKey client via WalletConnect, using OneKey to manage assets. [Refer to the WalletConnect documentation](../connect-to-software/using-walletconnect/).
3. Use a wallet aggregator SDK that supports OneKey.
   * [Web3 Onboard (EVM)](../connect-to-software/dapp-connect-to-onekey/compatible-with-metamask/third-party-wallet-kit/web3-onboard.md)
   * [Rainbowkit (EVM)](../connect-to-software/dapp-connect-to-onekey/compatible-with-metamask/third-party-wallet-kit/rainbowkit.md)

## Third-Party Wallet and App Developer Guide

he SDK access method is recommended, [see the SDK Access Details for more information](../connect-to-hardware/hardware-sdk/guide.md). You can also connect to OneKey via WalletConnect DeepLink, which requires the user to have the OneKey client installed. For details, [refer to the WalletConnect DeepLink documentation](https://docs.walletconnect.com/web3wallet/mobileLinking).

## Blockchain Developer Guide

* If you wish to integrate your network with OneKey, please choose one of the following methods:
  * Write code for OneKey software and hardware.
  * Contact the OneKey business team to discuss cooperation matters.

## How to Connect to OneKey

We currently support the following ways to connect to OneKey:

Provide a hardware SDK to link your App to OneKey hardware.

* Offer a hardware SDK to support various applications connecting to OneKey hardware. Supports multiple programming languages.
* OneKey Classic and OneKey Touch support USB and Bluetooth connections; OneKey Mini only supports USB.

### How DApps Connect to OneKey

OneKey supports cross-chain DApp access, ensuring full compatibility across different platforms. For details, [please refer to](../connect-to-software/dapp-connect-to-onekey/).

| Client                              | DApp support                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------- |
| OneKey Chrome plugin                | You can use DApp to connect to OneKey in Chrome.                                |
| OneKey Edge plugin                  | You can use DApp to connect to OneKey in Edge.                                  |
| OneKey Desktop（Windows、macOS、Linux） | You can use DApp to connect to OneKey in Built-in Browser on the Desktop.       |
| OneKey Mobile client（iOS、Android）   | You can use DApp to connect to OneKey in Built-in Browser on the Mobile client. |

### WalletConnect Connection Methods

We support two WalletConnect methods to connect to OneKey:

* DApp developers can connect to OneKey via the「WalletConnect QR code」.
* Mobile application client developers can use the 「WalletConnect DeepLink」 method.

#### Support for Scanning WalletConnect QR Code to Connect Your Application

* If you are using a DApp application, you can choose the WalletConnect connection method. Use the OneKey software wallet to scan the WalletConnect QR code to instantly connect to the DApp. You can then use the assets managed in OneKey to use the corresponding DApp.
* DApp developers who want to add WalletConnect connectivity to their projects to connect to OneKey can [refer to the relevant WalletConnect documentation](https://docs.walletconnect.com/quickstart).

#### Support for WalletConnect DeepLink to Link to OneKey

* The OneKey mobile client (iOS, Android) supports the WalletConnect DeepLink connection method.
* The premise is that the OneKey client is installed on the user's mobile phone,
* Your App can connect to the OneKey mobile client through WalletConnect's DeepLink, for details refer to the Demo and [related WalletConnect DeepLink documentation](https://docs.walletconnect.com/web3wallet/mobileLinking).

## How to Develop and Submit Application Code

As a developer, you can contribute code to OneKey. Please refer to the Guide for Developing and Submitting Code.
