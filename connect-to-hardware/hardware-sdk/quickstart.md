# Quickstart

## &#x20;Installation

{% tabs %}
{% tab title="USB" %}
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
{% endtabs %}

## Initialization

```typescript
import { HardwareSDK } from '@onekeyfe/hd-web-sdk';

HardwareSDK.init({
  debug: true,
  // The official iframe page deployed by OneKey
  // of course you can also deploy it yourself 
  connectSrc: 'https://jssdk.onekey.so/0.3.2/'
})
```

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
