# Getting Started

## Getting Started

To develop for OneKey Browser Extension or Mobile Apps, Please install it on your development machine. [Download here](https://onekey.so/plugin/).

{% hint style="info" %}
This guide assumes intermediate knowledge of HTML, CSS, and JavaScript.
{% endhint %}

Once OneKey is installed and running, you should find that new browser tabs have a `window.$onekey` object available in the developer console. This is how your website will interact with OneKey.

You can review the full API for that object here.

### Basic Considerations <a href="#basic-considerations" id="basic-considerations"></a>

#### Web3 Browser Detection <a href="#web3-browser-detection" id="web3-browser-detection"></a>

To verify if the browser is running OneKey Browser Extension, copy and paste the code snippet below in the developer console of your web browser:

```javascript
if (typeof window !== 'undefined' && window.$onekey) {  console.log('OneKey is installed!');}
```

You can review the full API for the `window.$onekey` object here.

#### Running a Test Network <a href="#running-a-test-network" id="running-a-test-network"></a>

In the top right menu of OneKey Browser Extension, select the network that you are currently connected to. Among several popular defaults, you'll find `Custom RPC` and `Localhost 8545`. These are both useful for connecting to a test blockchain, like [ganache](https://www.trufflesuite.com/ganache). You can quickly install and start Ganache if you have `npm` installed with `npm i -g ganache-cli && ganache-cli`.

Ganache has some great features for starting your application with different states. If your application starts with the `-m` flag, you can feed it the same seed phrase you have in your OneKey, and the test network will give each of your first 10 accounts 100 test ether, which makes it easier to start work.

Since your seed phrase is the power to control all your accounts, it is probably worth keeping at least one seed phrase for development, separate from any that you use for storing real value. One easy way to manage multiple seed phrases with OneKey is with multiple browser profiles, each of which can have its own clean extension installations.

**Resetting Your Local Nonce Calculation**

If you're running a test blockchain and restart it, you can accidentally confuse OneKey because it calculates the next [nonce](https://docs.onekey.so/en/Extension/Guide/sending-transactions.html#nonce-ignored) based on both the network state _and_ the known sent transactions.

To clear OneKey Browser Extension's transaction queue, and effectively reset its nonce calculation, you can use the `Reset Account` button in `Settings` (available in the top-right menu).

#### Detecting OneKey <a href="#detecting-onekey" id="detecting-onekey"></a>

If you want to differentiate OneKey Browser Extension from other ethereum-compatible browsers, you can detect OneKey Browser Extension using `window.$onekey`.

#### User State <a href="#user-state" id="user-state"></a>

Currently there are a few stateful things to consider when interacting with this API:

* What is the current network?
* What is the current account?

Both of these are available synchronously as `onekey.networkVersion` and `onekey.selectedAddress`. You can listen for changes using events too, see ([the API reference](http://127.0.0.1:5000/o/xrwuLsQQ99ktq7CL9Sjj/s/MLekF6KKtvSaULC7gUFY/)).

#### Connecting to OneKey <a href="#connecting-to-onekey" id="connecting-to-onekey"></a>

"Connecting" or "logging in" to OneKey effectively means "to access the user's Ethereum account(s)".

You should **only** initiate a connection request in response to direct user action, such as clicking a button. You should **always** disable the "connect" button while the connection request is pending. You should **never** initiate a connection request on page load.

We recommend that you provide a button to allow the user to connect OneKey to your dapp. Clicking this button should call the following method:

```javascript
window.$onekey.ethereum.request({ method: 'eth_requestAccounts' });
```

**Example:**

```javascript
<button class="enableEthereumButton">Enable Ethereum</button>
```

```javascript
const ethereumButton = document.querySelector('.enableEthereumButton');
ethereumButton.addEventListener('click', () => {  // Will Start the OneKey Browser Extension  onekey.request({ method: 'eth_requestAccounts' });});
```

This promise-returning function resolves with an array of hex-prefixed ethereum addresses, which can be used as general account references when sending transactions.

Over time, this method is intended to grow to include various additional parameters to help your site request all the setup it needs from the user during setup.

Since it returns a promise, if you're in an `async` function, you may log in like this:

```javascript
const accounts = await window.$onekey.ethereum.request({ method: 'eth_requestAccounts' });const account = accounts[0];// We currently only ever provide a single account,// but the array gives us some room to grow.
```

**Example:**

```javascript
<button class="enableEthereumButton">Enable Ethereum</button><h2>Account: <span class="showAccount"></span></h2>
```

```javascript
const ethereumButton = document.querySelector('.enableEthereumButton');const showAccount = document.querySelector('.showAccount');
ethereumButton.addEventListener('click', () => {  getAccount();});
async function getAccount() {  const accounts = await window.$onekey.ethereum.request({ method: 'eth_requestAccounts' });  const account = accounts[0];  showAccount.innerHTML = account;}
```

### Choosing a Convenience Library <a href="#choosing-a-convenience-library" id="choosing-a-convenience-library"></a>

Convenience libraries exist for a variety of reasons.

Some of them simplify the creation of specific user interface elements, some entirely manage the user account onboarding, and others give you a variety of methods of interacting with smart contracts, for a variety of API preferences, from promises, to callbacks, to strong types, and on.

The provider API itself is very simple, and wraps [Ethereum JSON-RPC](https://eth.wiki/json-rpc/API#json-rpc-methods) formatted messages, which is why developers usually use a convenience library for interacting with the provider, like [ethers](https://www.npmjs.com/package/ethers), [web3.js](https://www.npmjs.com/package/web3), [truffle](https://www.trufflesuite.com/), [Embark](https://framework.embarklabs.io/), or others. From those tools, you can generally find sufficient documentation to interact with the provider, without reading this lower-level API.
