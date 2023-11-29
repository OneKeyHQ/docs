# Provider API

OneKey Browser Extension injects a global API into websites visited by its users at `window.$onekey.ethereum`. This API allows websites to request users' Ethereum accounts, read data from blockchains the user is connected to, and suggest that the user sign messages and transactions. The presence of the provider object indicates an Ethereum user.

We recommend using `typeof window !== 'undefined' && window.$onekey.ethereum` to detect our provider in browser.

The Ethereum JavaScript provider API is specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).

```javascript
if (typeof window !== 'undefined' && window.$onekey?.ethereum) {  
  // From now on, this should always be true:  
  startApp(provider); // initialize your app
} else {  
  console.log('Please install OneKey Browser Extension at https://onekey.so/download!');
}
```

### Basic Usage <a href="#basic-usage" id="basic-usage"></a>

For any non-trivial Ethereum web application — a.k.a. dapp, web3 site etc. — to work, you will have to:

* Detect the OneKey provider (`window.$onekey.ethereum`)
* Detect which Ethereum network the user is connected to
* Get the user's Ethereum account(s)

The snippet at the top of this page is sufficient for detecting the provider. You can learn how to accomplish the other two by reviewing the snippet in the [Using the Provider section](provider-api.md).

The provider API is all you need to create a full-featured web3 application.

That said, many developers use a convenience library, such as [ethers](https://www.npmjs.com/package/ethers), instead of using the provider directly. If you are in need of higher-level abstractions than those provided by this API, we recommend that you use a convenience library.

### Chain IDs <a href="#chain-ids" id="chain-ids"></a>

These are the IDs of the Ethereum chains that OneKey supports by default. Visit [chainlist](https://chainlist.onekey.so/) for more.

| Hex  | Decimal | Network                         |
| ---- | ------- | ------------------------------- |
| 0x1  | 1       | Ethereum Main Network (Mainnet) |
| 0x38 | 56      | Binance Smart Chain (Mainnet)   |
| 0x80 | 128     | Huobi Eco Chain (Mainnet)       |
| 0x41 | 65      | OEC Mainnet                     |
| 0x89 | 137     | Matic (Polygon)                 |
| 0xfa | 250     | Fantom Main Network             |
| 0x64 | 100     | xDai Main Network               |
| 0x3  | 3       | Ropsten Testnet                 |
| 0x2a | 42      | kovan Testnet                   |
| 0x4  | 4       | Rinkeby Testnet                 |

### Methods <a href="#methods" id="methods"></a>

#### `window.$onekey.ethereum.isConnected()` <a href="#onekeyisconnected" id="onekeyisconnected"></a>

**Tip**

Note that this method has nothing to do with the user's accounts.

You may often encounter the word "connected" in reference to whether a web3 site can access the user's accounts. In the provider interface, however, "connected" and "disconnected" refer to whether the provider can make RPC requests to the current chain.

<pre class="language-javascript"><code class="lang-javascript"><strong>window.$onekey.ethereum.isConnected(): boolean;
</strong></code></pre>

Returns `true` if the provider is connected to the current chain, and `false` otherwise.

If the provider is not connected, the page will have to be reloaded in order for connection to be re-established. Please see the [`connect`](provider-api.md) and [`disconnect`](provider-api.md) events for more information.

#### `window.$onekey.ethereum.request(args)` <a href="#onekeyrequestargs" id="onekeyrequestargs"></a>

```javascript
interface RequestArguments {  method: string;  params?: unknown[] | object;}
window.$onekey.ethereum.request(args: RequestArguments): Promise<unknown>;
```

Use `request` to submit RPC requests to Ethereum via OneKey Browser Extension. It returns a `Promise` that resolves to the result of the RPC method call.

The `params` and return value will vary by RPC method. In practice, if a method has any `params`, they are almost always of type `Array<any>`.

If the request fails for any reason, the Promise will reject with an [Ethereum RPC Error](provider-api.md).

OneKey Browser Extension supports most standardized Ethereum RPC methods, in addition to a number of methods that may not be supported by other wallets.

See the OneKey Browser Extension RPC API documentation for details.

**Example**

```javascript
params: [  
    {    
        from: '0xb60e8dd61c5d32be8058bb8eb970870f0java7233155',    
        to: '0xd46e8dd67c5d32be8058bb8eb970870f07244567',    
        gas: '0x76c0', // 30400    
        gasPrice: '0x9184e72a000', // 10000000000000    
        value: '0x9184e72a', // 2441406250    
        data:      '0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675',  
     },
 ];
window.$onekey.ethereum.request({    
    method: 'eth_sendTransaction',    
    params,  
}).then((result) => {    
    // The result varies by by RPC method.    
    // For example, this method will return a transaction hash hexadecimal string on success.  
    })  
    .catch((error) => {    
        // If the request fails, the Promise will reject with an error.  
    });
```

### Events <a href="#events" id="events"></a>

The OneKey Browser Extension provider implements the [Node.js `EventEmitter`](https://nodejs.org/api/events.html) API.

This sections details the events emitted via that API.

There are innumerable `EventEmitter` guides elsewhere, but you can listen for events like this:

```javascript
window.$onekey.ethereum.on('accountsChanged', (accounts) => {  // Handle the new accounts, or lack thereof.  // "accounts" will always be an array, but it can be empty.});
window.$onekey.ethereum.on('chainChanged', (chainId) => {  // Handle the new chain.  // Correctly handling chain changes can be complicated.  // We recommend reloading the page unless you have good reason not to.  window.location.reload();});
```

#### connect <a href="#connect" id="connect"></a>

```javascript
interface ConnectInfo {  chainId: string;}
window.$onekey.ethereum.on('connect', handler: (connectInfo: ConnectInfo) => void);
```

The OneKey Browser Extension provider emits this event when it first becomes able to submit RPC requests to a chain.

We recommend using a `connect` event handler and the `window.$onekey.ethereum.isConnected()` method in order to determine when/if the provider is connected.

#### disconnect <a href="#disconnect" id="disconnect"></a>

```javascript
window.$onekey.ethereum.on('disconnect', handler: (error: ProviderRpcError) => void);
```

The OneKey provider emits this event if it becomes unable to submit RPC requests to any chain. In general, this will only happen due to network connectivity issues or some unforeseen error.

Once `disconnect` has been emitted, the provider will not accept any new requests until the connection to the chain has been re-restablished, which requires reloading the page. You can also use the `window.$onekey.ethereum.isConnected()` method to determine if the provider is disconnected.

#### accountsChanged <a href="#accountschanged" id="accountschanged"></a>

```javascript
window.$onekey.ethereum.on('accountsChanged', handler: (accounts: Array<string>) => void);
```

The OneKey provider emits this event whenever the return value of the `eth_accounts` RPC method changes. `eth_accounts` returns an array that is either empty or contains a single account address. The returned address, if any, is the address of the most recently used account that the caller is permitted to access. Callers are identified by their URL _origin_, which means that all sites with the same origin share the same permissions.

This means that `accountsChanged` will be emitted whenever the user's exposed account address changes.



**Tip**

We plan to allow the `eth_accounts` array to be able to contain multiple addresses in the near future.

#### chainChanged <a href="#chainchanged" id="chainchanged"></a>



**Tip**

See the [Chain IDs section](provider-api.md) for OneKey Browser Extension's default chains and their chain IDs.

```javascript
window.$onekey.ethereum.on('chainChanged', handler: (chainId: string) => void);
```

The OneKey Browser Extension provider emits this event when the currently connected chain changes.

All RPC requests are submitted to the currently connected chain. Therefore, it's critical to keep track of the current chain ID by listening for this event.

<pre class="language-javascript"><code class="lang-javascript"><strong>window.$onekey.ethereum.on('chainChanged', (_chainId) => window.location.reload());
</strong></code></pre>

#### message <a href="#message" id="message"></a>

```javascript
interface ProviderMessage {  type: string;  data: unknown;}
window.$onekey.ethereum.on('message', handler: (message: ProviderMessage) => void);
```

The OneKey provider emits this event when it receives some message that the consumer should be notified of.

The kind of message is identified by the `type` string.

RPC subscription updates are a common use case for the `message` event.

For example, if you create a subscription using `eth_subscribe`, each subscription update will be emitted as a `message` event with a `type` of `eth_subscription`.

### Errors <a href="#errors" id="errors"></a>

All errors thrown or returned by the OneKey Browser Extension provider follow this interface:

```javascript
interface ProviderRpcError extends Error {  message: string;  code: number;  data?: unknown;}
```

The `window.$onekey.ethereum.request(args)` method throws errors eagerly.

You can often use the error `code` property to determine why the request failed.

Common codes and their meaning include:

* `4001`
  * The request was rejected by the user
* `-32602`
  * The parameters were invalid
* `-32603`
  * Internal error

For the complete list of errors, please see [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193#provider-errors) and [EIP-1474](https://eips.ethereum.org/EIPS/eip-1474#error-codes).
