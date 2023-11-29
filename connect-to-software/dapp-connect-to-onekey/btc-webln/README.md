# WebLN

OneKey Browser Extension injects a global API into websites visited by its users at `window.$onekey.webln`.  Using WebLN API is straightforward. WebLN offers a comprehensive set of APIs designed for building Bitcoin Lightning-driven web applications using the WebLN standard and requires just a few lines of code.

We recommend using `typeof window !== 'undefined' && window.$onekey.webln` to detect our provider in browser.

```javascript
if (typeof window !== 'undefined' && window.$onekey?.webln) {  
  // From now on, this should always be true:  
  startApp(provider); // initialize your app
} else {  
  console.log('Please install OneKey Browser Extension at https://onekey.so/download!');
}
```
