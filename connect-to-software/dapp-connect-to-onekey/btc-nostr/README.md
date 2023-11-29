# BTC Nostr

OneKey Browser Extension injects a global API into websites visited by its users at `window.$onekey.nostr`. The `window.$onekey.nostr` object may be made available by web browsers or extensions and websites or web-apps may make use of it after checking its availability.

We recommend using `typeof window !== 'undefined' && window.$onekey.nostr` to detect our provider in browser.

The JavaScript provider API is specified by [NIPS-07](https://github.com/nostr-protocol/nips/blob/master/07.md).

```javascript
if (typeof window !== 'undefined' && window.$onekey?.nostr) {  
  // From now on, this should always be true:  
  startApp(provider); // initialize your app
} else {  
  console.log('Please install OneKey Browser Extension at https://onekey.so/download!');
}
```
