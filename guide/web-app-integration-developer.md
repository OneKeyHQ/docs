# Web App Developer

## &#x20;Started

To develop for OneKey Browser Extension or Mobile Apps, Please install it on your development machine. [Download here.](https://onekey.so/download?client=browserExtension)

{% hint style="info" %}
This guide assumes intermediate knowledge of HTML, CSS, and JavaScript.
{% endhint %}

Once OneKey is installed and running, you should find that new browser tabs have a `window.$onekey` object available in the developer console. This is how your website will interact with OneKey.

## Intro

As a DApp developer, you have the following options for connecting to OneKey:

## OneKey Provider API

OneKey injects the Provider API into websites visited by users, allowing these websites to request relevant APIs to interact with blockchains through OneKey. e.g [Ethereum Provider API](../connect-to-software/webapp-connect-onekey/eth/provider-api.md).

OneKey provides Provider APIs for many blockchains.

{% content-ref url="../connect-to-software/webapp-connect-onekey/" %}
[webapp-connect-onekey](../connect-to-software/webapp-connect-onekey/)
{% endcontent-ref %}



Below are scenarios for different clients using the Provider API.

| Client                              | DApp support                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------- |
| OneKey Chrome plugin                | You can use DApp to connect to OneKey in Chrome.                                |
| OneKey Edge plugin                  | You can use DApp to connect to OneKey in Edge.                                  |
| OneKey Desktop（Windows、macOS、Linux） | You can use DApp to connect to OneKey in Built-in Browser on the Desktop.       |
| OneKey Mobile client（iOS、Android）   | You can use DApp to connect to OneKey in Built-in Browser on the Mobile client. |



## Use Wallet aggregator GUIs and SDKs

Below are the wallet aggregator GUIs and SDKs that already support OneKey. If you are looking for a GUI or SDK, consider using one of the following aggregators that support OneKey. If you have already integrated with one of these aggregators, you can also refer to the documentation for better integration with OneKey.

* Metamask SDK  [Read more >>>](../connect-to-software/compatible-with-metamask/)
* Web3 Onboard [Read more >>>](../connect-to-software/support-wallet-kit/web3-onboard.md)
* Rainbowkit [Read more >>>](../connect-to-software/support-wallet-kit/rainbowkit.md)

## WalletConnect Integration

If you are using a DApp application, you can choose the WalletConnect connection method. Use the OneKey software wallet to scan the WalletConnect QR code to instantly connect to the DApp. You can then use the assets managed in OneKey to use the corresponding DApp.

* DApp developers who want to add WalletConnect connectivity to their projects to connect to OneKey can [refer to the relevant WalletConnect documentation](https://docs.walletconnect.com/quickstart).

