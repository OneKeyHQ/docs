# BTC Unisat

OneKey Browser Extension injects a global API into websites visited by its users at `window.$onekey.btc`. The `window.$onekey.btc` object may be made available by web browsers or extensions and websites or web-apps may make use of it after checking its availability.

We recommend using `typeof window !== 'undefined' && window.$onekey.btc` to detect our provider in browser.

```javascript
if (typeof window !== 'undefined' && window.$onekey?.btc) {  
  // From now on, this should always be true:  
  startApp(provider); // initialize your app
} else {  
  console.log('Please install OneKey Browser Extension at https://onekey.so/download!');
}
```

