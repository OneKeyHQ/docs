# Install the SDK

## Installation

Connection options for the device models covered by this guide:

| Device            | Bluetooth            | USB                  |
| ----------------- | -------------------- | -------------------- |
| OneKey Classic    | :white\_check\_mark: | :white\_check\_mark: |
| OneKey Classic 1s | :white\_check\_mark: | :white\_check\_mark: |
| OneKey Mini       | :x:                  | :white\_check\_mark: |
| OneKey Touch      | :white\_check\_mark: | :white\_check\_mark: |
| OneKey Pro        | :white\_check\_mark: | :white\_check\_mark: |

Choose the package for your runtime and transport. The table below describes the integrations covered by this guide.

<table><thead><tr><th width="324.3333333333333">Platform</th><th>Bluetooth</th><th>USB</th></tr></thead><tbody><tr><td>TypeScript、JavaScript（Web environment）</td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-web-sdk">@onekeyfe/hd-web-sdk</a></td></tr><tr><td>React Native</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-ble-sdk">@onekeyfe/hd-ble-sdk</a></td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td></tr><tr><td>Android</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td></tr><tr><td>iOS</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td></tr><tr><td>Flutter</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td></tr></tbody></table>

{% tabs %}
{% tab title="Web" %}
```shell
# Install via NPM
npm install --save @onekeyfe/hd-web-sdk

# Install via YARN
yarn add @onekeyfe/hd-web-sdk
```

## Initialization

```
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

await HardwareSDK.init({
  debug: true,
  fetchConfig: true,
  connectSrc: 'https://jssdk.onekey.so/1.0.38/'
})
```

**fetchConfig:** Allows querying for updated device version information over the network, used for prompting device updates and informing which version is needed for older hardware to use new features.

**connectSrc**: The official web page deployed by OneKey is used to create an iframe on the page to communicate with OneKey Bridge.

The complete link to the web page is `https://jssdk.onekey.so/1.0.38/iframe.html`.

Normally, the number after the URL should match the version number of the SDK you installed. For example, “1.0.38” in this case.

If encountering issues with the web page failing to load for the corresponding version number, please try using a different version of the SDK or submit an issue on [GitHub](https://github.com/OneKeyHQ/hardware-js-sdk/issues) for feedback. Include the SDK version and failing iframe URL in the report.
{% endtab %}

{% tab title="React Native" %}
```bash
# Install via NPM
npm install --save @onekeyfe/hd-ble-sdk

# Install via YARN
yarn add @onekeyfe/hd-ble-sdk
```

## Initialization

```javascript
import HardwareSDK from '@onekeyfe/hd-ble-sdk';

await HardwareSDK.init({
  debug: true,
  fetchConfig: true,
})
```

**fetchConfig:** Allows querying for updated device version information over the network, used for prompting device updates and informing which version is needed for older hardware to use new features.
{% endtab %}

{% tab title="Nodejs / iOS Native / Android Native / Flutter" %}
For native and Flutter integrations, run the common SDK in a JavaScript runtime and provide a transport plugin that communicates with native code.

* First, you need a container that can run JavaScript and communicate with it.
* Prepare the JavaScript part of the code, and install @onekeyfe/hd-common-connect-sdk.

```bash
# Install via NPM
npm install --save @onekeyfe/hd-common-connect-sdk

# Install via YARN
yarn add @onekeyfe/hd-common-connect-sdk
```

Then, handle the part that communicates with the hardware device.

For specific platforms, how to integrate hd-common-connect-sdk, and demos for relevant platforms, [please visit to learn more](advanced/common-sdk-guide.md).
{% endtab %}
{% endtabs %}

Next, you can return to the [Quickstart](started.md) and proceed with the steps for [Configuring the Event](config-event.md).
