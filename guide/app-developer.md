---
description: Third-Party Wallet and App Developer
---

# App Developer

## Intro

* If you want to connect hardware, please use our SDK.
* If you want to connect to the software, we support WalletConnect Deeplink method to connect.



## Connect OneKey hardware

### Online debugging tool

#### Step 1: Install bridge

The web terminal can communicate with hardware only after a hardware bridge is installed.

[Install Hardware Bridge](https://onekey.so/download?client=bridge)

#### Setp 2: Debug

You can use a USB connection to the device for debugging APIs.

[Online Debugging Tool](https://hardware-example.onekeytest.com/)



### Integrated Hardware SDK

Offer a hardware SDK to support various applications connecting to OneKey hardware. Supports multiple programming languages.

The specific steps for integrating the SDK. [Read more >>>](../connect-to-hardware/hardware-sdk/started.md)

## WalletConnect Integration

We support WalletConnect Deeplink, enabling you to directly connect to OneKey via a deeplink, and then communicate with the blockchain through OneKey.

* The OneKey mobile client (iOS, Android) supports the WalletConnect DeepLink connection method.
* The premise is that the OneKey client is installed on the user's mobile phone,
* Your App can connect to the OneKey mobile client through WalletConnect's DeepLink, for details refer to the Demo and [related WalletConnect DeepLink documentation](https://docs.walletconnect.com/web3wallet/mobileLinking).
