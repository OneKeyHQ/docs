# Migration from MetaMask

If there is already a Dapp Web Application compatible with the latest MetaMask (Metamask v8 or later) before, it will be very easy to be compatible with the OneKey browser Extension.

The latest MetaMask will inject the `window.ethereum` object to make the page manipulate the user's account in the metamask and obtain configuration information. Similarly, while `window.onekey` maintains a large number of APIs compatible with `window.ethereum`, and OneKey Browser Extension also injects `window.ethereum` to ensure the compatibility of other Dapps. Users can manually turn on "Alternative MetaMask" in OneKey browser Extension v2.0.1 or later to prevent possible conflicts during co-installation with MetaMask.

### Best Practices <a href="#best-practices" id="best-practices"></a>

It is recommended that Dapp developers completely distinguish the two objects of `window.onekey` and `window.ethereum` when calling internal methods, namely:

* If you want the logic triggered by the OneKey browser Extension will completely use `window.onekey` for subsequent operations.
* If you want the logic triggered by the MetaMask browser Extension will completely use `window.ethereum` for subsequent operations.At the same time, through the following JS code, you can ensure that the user wakes up MetaMask.

```javascript
// If you want to operate the methods and logic of MetaMask related providersif (window.ethereum && window.ethereum.switchProvider) {  window.ethereum.switchProvider('metamask');}// After the switchProvider is executed, the subsequent operation window.ethereum must be the metamask injected into the page
```

### Other options <a href="#other-options" id="other-options"></a>

Of course, in order to be compatible with earlier Dapps that are not compatible with `window.onekey`, the OneKey browser Extension will also add `window.ethereum` to the page to assist in operations.

Use the `window.ethereum.switchProvider` method to switch between the variables injected by the OneKey browser Extension and the variables injected by MetaMask.



### **warning**

`window.ethereum.switchProvider` is the new content of OneKey browser plugin v2.0.1 and later, and it is also a unique method of OneKey browser Extension.

If the user does not install the OneKey browser Extension and only installs MetaMask, the method does not exist. Please make sure that the method is accessible before calling.

```javascript
// connect MetaMaskfunction showInstallMessage(){  alert('Please Install MetaMask here: https://metamask.io/');}
if (!window.ethereum) {  showInstallMessage();  return;}
// DO NOT call methods or accessing properties before switchProvider()if (window.ethereum && window.ethereum.switchProvider) {  window.ethereum.switchProvider('metamask');}
if (!window.ethereum.isMetaMask) {  showInstallMessage();  return;}
// call window.ethereum methods...
```

```javascript
// connect OneKeyfunction showInstallMessage(){  alert('Please Install MetaMask here: https://onekey.so/');}
if (!window.ethereum) {  showInstallMessage();  return;}
// DO NOT call methods or accessing properties before switchProvider()if (window.ethereum && window.ethereum.switchProvider) {  window.ethereum.switchProvider('onekey');}
if (!window.ethereum.isOneKey) {  showInstallMessage();  return;}
// call window.ethereum methods...
```

{% hint style="warning" %}
Developers must ensure that all relevant business codes that access `window.ethereum` must be executed after switchProvider.

Otherwise, the code executed before switchProvider may cause an exception because of the uncertainty of the extension instance corresponding to `window.ethereum`.
{% endhint %}
