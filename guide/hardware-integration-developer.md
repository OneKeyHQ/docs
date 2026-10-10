---
description: Third-Party Wallet and App Developer
---

# Connect hardware

Use the Hardware SDK when your wallet or application needs to communicate directly with a OneKey device. Your application prepares requests, manages connection state, and handles SDK UI events; the user reviews sensitive operations on the device.

For current platform setup and device support, use the [Hardware SDK quickstart](https://developer.onekey.so/en/hardware-sdk/getting-started/).

## Choose a transport

| Runtime                         | Integration guide                                                                           |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| Browser with WebUSB support     | [WebUSB](https://developer.onekey.so/en/hardware-sdk/transport/web-usb/)                    |
| React Native with Bluetooth     | [React Native BLE](https://developer.onekey.so/en/hardware-sdk/transport/react-native-ble/) |
| Native Android, iOS, or Flutter | [Native mobile](https://developer.onekey.so/en/hardware-sdk/transport/native-ble/)          |

Bridge-based setups are a separate integration path. A hardware bridge is not a prerequisite for every browser integration; follow the requirements of the transport you select.

## Make your first call

1. Install the SDK for your runtime and initialize it once.
2. Register UI handlers before starting a device call.
3. Discover devices, handle an empty result, and let the user select the intended device.
4. Read device features and retrieve an address.
5. Add signing only after you handle rejection, cancellation, and disconnects.

Use the [hardware playground](https://hardware-example.onekey.so/) to explore APIs, then validate with the device and firmware your application supports. Consult [device and chain support](https://developer.onekey.so/en/hardware-sdk/concepts/chain-support/) before selecting a method.

## QR signing

For QR-capable devices, use the [Air-Gap SDK](../connect-to-hardware/air-gap-sdk/) to exchange requests and responses. This path does not use a USB or Bluetooth connection.

## Connecting to the OneKey app instead

If you want the OneKey wallet app to manage accounts and signing, use the [dApp integration guide](web-app-integration-developer.md), including its mobile and WalletConnect paths.
