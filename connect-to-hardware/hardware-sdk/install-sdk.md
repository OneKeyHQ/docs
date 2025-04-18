# Install SDK

## &#x20;Installation

The support status for Bluetooth and USB on different devices.

| Device            | Bluetooth            | USB                  |
| ----------------- | -------------------- | -------------------- |
| OneKey Classic    | :white\_check\_mark: | :white\_check\_mark: |
| OneKey Classic 1s | :white\_check\_mark: | :white\_check\_mark: |
| OneKey Mini       | :x:                  | :white\_check\_mark: |
| OneKey Touch      | :white\_check\_mark: | :white\_check\_mark: |
| Onekey Pro        | :white\_check\_mark: | :white\_check\_mark: |



This is the platform we support.

<table><thead><tr><th width="324.3333333333333">Platform</th><th>Bluetooth</th><th>USB</th></tr></thead><tbody><tr><td>TypeScript、JavaScript（Web environment）</td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-web-sdk">@onekeyfe/hd-web-sdk</a></td></tr><tr><td>React Native</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-ble-sdk">@onekeyfe/hd-ble-sdk</a></td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td></tr><tr><td>Android</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td></tr><tr><td>iOS</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><span data-gb-custom-inline data-tag="emoji" data-code="274c">❌</span></td></tr><tr><td>Flutter</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/tree/onekey/packages/hd-common-connect-sdk">@onekeyfe/hd-common-connect-sdk</a></td></tr></tbody></table>

{% tabs %}
{% tab title="Web" %}
<pre class="language-shell"><code class="lang-shell"><strong># Install via NPM
</strong>npm install --save @onekeyfe/hd-web-sdk

# Install via YARN
yarn add @onekeyfe/hd-web-sdk
</code></pre>

## Initialization

```
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

HardwareSDK.init({
  debug: true,
  fetchConfig: true,
  connectSrc: 'https://jssdk.onekey.so/0.3.38/'
})
```

**fetchConfig:** Allows querying for updated device version information over the network, used for prompting device updates and informing which version is needed for older hardware to use new features.

**connectSrc**: The official web page deployed by OneKey is used to create an iframe on the page to communicate with OneKey Bridge.&#x20;

The complete link to the web page is `https://jssdk.onekey.so/0.3.38/iframe.html`.

Normally, the number after the URL should match the version number of the SDK you installed. For example, “0.3.38” in this case.

If encountering issues with the web page failing to load for the corresponding version number, please try using a different version of the SDK or submit an issue on [GitHub](https://github.com/OneKeyHQ/hardware-js-sdk/issues) for feedback. We will work quickly to resolve the problem.
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
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-ble-sdk';

HardwareSDK.init({
  debug: true,
  fetchConfig: true,
})
```

**connectSrc**: The official web page deployed by OneKey is used to create an iframe on the page to communicate with OneKey Bridge.&#x20;

**fetchConfig:** Allows querying for updated device version information over the network, used for prompting device updates and informing which version is needed for older hardware to use new features.
{% endtab %}

{% tab title="Nodejs / iOS Native / Android Native / Flutter" %}
Because our SDK is developed using the RN (React Native) and JS (JavaScript) technology stack. To avoid requiring everyone to depend on React Native, we adopt the following solution.&#x20;

* First, you need a container that can run JavaScript and communicate with it.&#x20;
* Prepare the JavaScript part of the code, and install @onekeyfe/hd-common-connect-sdk.

<pre class="language-bash"><code class="lang-bash"><strong># Install via NPM
</strong>npm install --save @onekeyfe/hd-common-connect-sdk

# Install via YARN
yarn add @onekeyfe/hd-common-connect-sdk
</code></pre>

Then, handle the part that communicates with the hardware device.

For specific platforms, how to integrate hd-common-connect-sdk, and demos for relevant platforms, [please visit to learn more](advanced/common-sdk-guide.md).
{% endtab %}
{% endtabs %}

Next, you can return to the [Quickstart](started.md) and proceed with the steps for [Configuring the Event](config-event.md).
