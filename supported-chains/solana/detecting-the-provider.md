# Detecting the Provider

To detect if a user has already installed OneKey, a web application should check for the existence of a `window.$onekey.solana` object. OneKey Browser Extension, OneKey mobile APP and OneKey Desktop in-app browser will both inject a `$onekey.solana` object into the `window` of any web application the user visits.

If a `window.$onekey.solana` object exists, Solana apps can interact with OneKey via the API found at `window.$onekey.solana`. This `solana` object is also available at `window.solana` to support legacy integrations.&#x20;

To detect if OneKey is installed, an application should check for an additional `isOneKey` flag.

```javascript
const isOneKeyInstalled = window.$onekey?.solana?.isOneKey
```
