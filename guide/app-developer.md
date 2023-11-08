---
description: Third-Party Wallet and App Developer
---

# App Developer

## Intro

* If you want to connect hardware, please use our SDK.
* If you want to connect to the software, we support WalletConnect Deeplink method to connect.

## Connect OneKey hardware

* A hardware SDK is available to connect your app with OneKey hardware.
* It supports various programming languages and can work with different OneKey hardware models.
* OneKey Classic and OneKey Touch models support both USB and Bluetooth connections, while the OneKey Mini is limited to USB.

Provide a hardware SDK to link your App to OneKey hardware.

* Offer a hardware SDK to support various applications connecting to OneKey hardware. Supports multiple programming languages.
* OneKey Classic and OneKey Touch support USB and Bluetooth connections; OneKey Mini only supports USB.

[Read more >>>](../connect-to-hardware/hardware-sdk/started.md)

### Headware Online debugging tool

#### Step 1: Install bridge

The web terminal can communicate with hardware only after a hardware bridge is installed.

[Install Hardware Bridge](https://onekey.so/download?client=bridge)

#### Setp 2: Debug

[Online Debugging Tool](https://hardware-example.onekeytest.com/)



## Support for WalletConnect DeepLink to Link to OneKey

* The OneKey mobile client (iOS, Android) supports the WalletConnect DeepLink connection method.
* The premise is that the OneKey client is installed on the user's mobile phone,
* Your App can connect to the OneKey mobile client through WalletConnect's DeepLink, for details refer to the Demo and [related WalletConnect DeepLink documentation](https://docs.walletconnect.com/web3wallet/mobileLinking).
