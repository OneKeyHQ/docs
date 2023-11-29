# Detecting the Provider

To detect if a user has already installed OneKey, a web application should check for the existence of a `window.$onekey.solana` object.

OneKey Browser Extension, OneKey Mobile APP and OneKey Desktop in-app browser will both inject a `$onekey.solana` object into the `window` of any web application the user visits.

If a `window.$onekey.solana` object exists, Solana apps can interact with OneKey via the API found at `window.$onekey.solana`. This `solana` object is also available at `window.solana` to support legacy integrations.&#x20;

To detect if OneKey is installed, an application should check for an additional `isOneKey` flag.

```javascript
const isOneKeyInstalled = window.$onekey?.solana?.isOneKey;
```

If OneKey is not installed, we recommend you redirect your users to [https://onekey.so/download/?client=browserExtension](https://onekey.so/zh\_CN/download/?client=browserExtension). Altogether, this may look like the following.

```javascript
const getProvider = () => {
  if ('$onekey' in window) {
    const provider = window.$onekey?.solana;

    if (provider?.isOneKey) {
      return provider;
    }
  }

  window.open('https://onekey.so/download/?client=browserExtension', '_blank');
};
```

