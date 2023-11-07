# Quickstart

## &#x20;Installation

This is the platform we support.

<table><thead><tr><th width="324.3333333333333"></th><th>蓝牙</th><th>USB</th></tr></thead><tbody><tr><td>TypeScript、JavaScript（Web environment）</td><td>X</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-web-sdk">@onekeyfe/hd-web-sdk</a></td></tr><tr><td>React Native</td><td><a href="https://github.com/OneKeyHQ/hardware-js-sdk/blob/onekey/packages/hd-ble-sdk">@onekeyfe/hd-ble-sdk</a></td><td>X</td></tr><tr><td>Android（Java、Kotlin）</td><td>Comingg soon</td><td>Coming soon</td></tr><tr><td>iOS（Swift、Objective-C）</td><td>Coming soon</td><td>Coming soon</td></tr><tr><td>Flutter</td><td>Coming soon</td><td>Coming soon</td></tr></tbody></table>



{% tabs %}
{% tab title="Web" %}
<pre class="language-shell"><code class="lang-shell"><strong># Install via NPM
</strong>npm install --save @onekeyfe/hd-web-sdk

# Install via YARN
yarn add @onekeyfe/hd-web-sdk
</code></pre>
{% endtab %}

{% tab title="Bluetooth" %}
```shell
# Install via NPM
npm install --save @onekeyfe/hd-ble-sdk

# Install via YARN
yarn add @onekeyfe/hd-ble-sdk
```
{% endtab %}

{% tab title="Nodejs / iOS Native / Android Native / Flutter" %}
```bash
bash# Install via NPM
npm install --save @onekeyfe/hd-common-connect-sdk

# Install via YARN
yarn add @onekeyfe/hd-common-connect-sdk
```
{% endtab %}
{% endtabs %}

## Initialization

```typescript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

HardwareSDK.init({
  debug: true,
  connectSrc: 'https://jssdk.onekey.so/0.3.30/'
})
```

**connectSrc**: The official web page deployed by OneKey is used to create an iframe on the page to communicate with OneKey Bridge.&#x20;

The complete link to the web page is [`https://jssdk.onekey.so/0.3.30/iframe.html`](https://jssdk.onekey.so/0.3.30/iframe.html).

Normally, the number after the URL should match the version number of the SDK you installed. For example, “0.3.30” in this case.

If encountering issues with the web page failing to load for the corresponding version number, please try using a different version of the SDK or submit an issue on [GitHub](https://github.com/OneKeyHQ/hardware-js-sdk/issues) for feedback. We will work quickly to resolve the problem.

## API Reference

* [Reference](api-reference/)

## Event

* [Reference](event.md)

## Development

```shell
$ git clone https://github.com/OneKeyHQ/hardware-js-sdk.git
$ cd hardware-js-sdk
$ yarn && yarn bootstrap

# build
yarn build

# start hd-web-sdk
cd packages/hd-web-sdk && yarn dev

```
