# Install the Provider SDK

{% hint style="info" %}
Provider SDK works with [OneKey Browser Extension](https://www.onekey.so/plugin) ( version >= 2.3.2 ) installed
{% endhint %}

We provide an SDK named [onekey-near-provider](https://www.npmjs.com/package/@onekeyfe/onekey-near-provider) as an npm package, which also requires [near-api-js](https://www.npmjs.com/package/near-api-js) installed.&#x20;

You can check the sdk source code at [Github](https://github.com/OneKeyHQ/cross-inpage-provider/tree/master/packages/providers/onekey-near-provider), with full example code [here](https://github.com/OneKeyHQ/cross-inpage-provider/tree/master/packages/example).

```bash
# with npm
npm install near-api-js @onekeyfe/onekey-near-provider

# with yarn
yarn add near-api-js @onekeyfe/onekey-near-provider

```

Then you can create the provider instance.

```javascript
import { OneKeyNearProvider } from '@onekeyfe/onekey-near-provider';

const provider = new OneKeyNearProvider();

```

{% hint style="warning" %}
**Caution:** The Provider SDK is not compatible with SSR.

If you are using [next.js](https://nextjs.org/) including SSR feature, you can check this document about how to  [dynamic import with no ssr](https://nextjs.org/docs/advanced-features/dynamic-import#with-no-ssr).
{% endhint %}
