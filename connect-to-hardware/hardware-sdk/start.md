# Start



## Install

{% tabs %}
{% tab title="USB" %}
<pre class="language-shell"><code class="lang-shell"><strong># Install via NPM
</strong>npm install --save @onekeyfe/hd-web-sdk

# Install via YARN
yarn add @onekeyfe/hd-web-sdk</code></pre>
{% endtab %}

{% tab title="Bluetooth" %}
```shell

# Install via NPM
npm install --save @onekeyfe/hd-ble-sdk

# Install via YARN
yarn add @onekeyfe/hd-ble-sdk
```
{% endtab %}
{% endtabs %}

## Initialization

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import HardwareSDK from '@onekeyfe/hd-web-sdk';

HardwareSDK.init({
  debug: true,
  connectSrc: 'https://jssdk.onekey.so/0.1.26/' // 部署的 iframe 页面
})
```
{% endtab %}
{% endtabs %}



## API Methods

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
