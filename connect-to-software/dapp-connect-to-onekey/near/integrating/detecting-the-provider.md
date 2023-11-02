# Detecting the Provider

You can call some of the provider methods only if OneKey extension is installed on the end user browser.

To detect if extension is installed, you can call the aysnc method `detectWalletInstalled`.

If OneKey is not installed, we recommend you redirect your users to [our website](https://www.onekey.so). Altogether, this may look like the following.

```javascript
const installed = await provider.detectWalletInstalled();
if (installed) {
  await provider.request({ method: 'near_accounts' });
} else {
  window.open('https://www.onekey.so');
}
```
