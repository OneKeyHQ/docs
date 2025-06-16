# Provider API

OneKey Browser Extension injects a global API into websites visited by its users at `window.$onekey.ethereum`. This API allows websites to request users' Ethereum accounts, read data from blockchains the user is connected to, and suggest that the user sign messages and transactions. The presence of the provider object indicates an Ethereum user.

The Ethereum JavaScript provider API is specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193). For modern applications, it is highly recommended to use the [EIP-6963](https://eips.ethereum.org/EIPS/eip-6963) standard for discovering multiple wallet providers, which avoids conflicts between different browser extensions.

### Provider Detection

To ensure compatibility and a smooth user experience in a multi-wallet environment, it is highly recommended to detect providers using the **EIP-6963** standard.

#### Recommended: EIP-6963 for Multi-Wallet Support

OneKey fully supports **EIP-6963**, which allows dApps to discover multiple installed wallet providers without conflicts. This is the modern, recommended approach for all dApps.

```javascript
// This code snippet demonstrates how to listen for announced providers
// and store them for your dApp to use.

let onekeyProvider = null;

const onAnnounceProvider = (event) => {
  // The event.detail contains the provider info and the provider object itself.
  // event.detail = { info: { uuid, name, icon, rdns }, provider }
  if (event.detail.info.name === 'OneKey') {
    onekeyProvider = event.detail.provider;
  }
  // You can also store all providers in an array to let users choose.
};

// Listen for the announcement event from all wallets
window.addEventListener('eip6963:announceProvider', onAnnounceProvider);

// It's also good practice to dispatch a request event to prompt wallets
// that may have loaded after the initial page load.
window.dispatchEvent(new Event('eip6963:requestProvider'));

// After a short delay, you can check if the onekeyProvider was found.
setTimeout(() => {
  if (onekeyProvider) {
    startApp(onekeyProvider);
  } else {
    console.log('OneKey Wallet not found. Please install it from onekey.so/download');
  }
}, 500);
```

#### Legacy Detection & Provider Objects

For compatibility with older dApps, OneKey also injects its provider into `window.ethereum`.

* `window.ethereum`: This is the commonly used provider object. However, if a user has multiple wallets installed, this variable can be overwritten, leading to conflicts. OneKey injects here to maximize compatibility.
* `window.$onekey.ethereum`: To avoid conflicts, OneKey also provides its provider via this private, namespaced variable. This ensures a dApp can specifically target OneKey if needed, though using EIP-6963 is preferred.

### Basic Usage

For any non-trivial Ethereum web application (dApp) to work, you will need to accomplish three fundamental tasks:

1. **Detect an Ethereum Provider:** You must first find out if the user has a wallet like OneKey installed. The recommended way to do this is by using the **EIP-6963** standard, as detailed in the **Provider Detection** section above.
2. **Access User Accounts:** Once a provider is detected, you need to request permission to view the user's accounts. This is almost always done by calling the `eth_requestAccounts` method on the provider object.
3. **Monitor Network Changes:** The user can switch between different networks (like Ethereum Mainnet and Sepolia) at any time. Your dApp must listen for the `chainChanged` event to stay aware of the currently connected network.

The provider object is the essential tool for building a full-featured Web3 application. While you can use the provider API directly for all interactions, many developers prefer using a convenience library to simplify development. We recommend robust, community-vetted libraries like [**ethers.js**](https://ethers.io/) or [**viem**](https://viem.sh/), which provide higher-level abstractions and utilities.

### Chain IDs

These are the IDs of the Ethereum chains that OneKey supports by default. Visit [chainlist.org](https://chainlist.org/) for a comprehensive list.

| Hex      | Decimal  | Network                       | Status |
| -------- | -------- | ----------------------------- | ------ |
| 0x1      | 1        | Ethereum Main Network         | Live   |
| 0x38     | 56       | Binance Smart Chain (Mainnet) | Live   |
| 0x80     | 128      | Huobi Eco Chain (Mainnet)     | Live   |
| 0x89     | 137      | Polygon (Matic) Mainnet       | Live   |
| 0xfa     | 250      | Fantom Main Network           | Live   |
| 0xaa36a7 | 11155111 | Sepolia Testnet               | Active |

### Methods

Once you have obtained a provider object (e.g., via EIP-6963), you can use the following EIP-1193 methods to interact with the user's wallet.

#### `eth_requestAccounts`

Use `eth_requestAccounts` to request one or more accounts from the user's wallet. Calling this will typically prompt the user with a connection request.

```javascript
try {
  const accounts = await window.$onekey.ethereum.request({ method: 'eth_requestAccounts' });
  // handle accounts
} catch (error) {
  // handle error, e.g., user rejected request
}
```

#### `window.$onekey.ethereum.isConnected()`

> **Tip**\
> Note that this method has nothing to do with the user's accounts.\
> You may often encounter the word "connected" in reference to whether a web3 site can access the user's accounts. In the provider interface, however, "connected" and "disconnected" refer to whether the provider can make RPC requests to the current chain.

```typescript
window.$onekey.ethereum.isConnected(): boolean;
```

Returns `true` if the provider is connected to the current chain, and `false` otherwise.

If the provider is not connected, the page will have to be reloaded in order for connection to be re-established. Please see the `connect` and `disconnect` events for more information.

#### `window.$onekey.ethereum.request(args)`

```typescript
interface RequestArguments {
  method: string;
  params?: unknown[] | object;
}

window.$onekey.ethereum.request(args: RequestArguments): Promise<unknown>;
```

Use `request` to submit RPC requests to Ethereum via OneKey Browser Extension. It returns a `Promise` that resolves to the result of the RPC method call.

The `params` and return value will vary by RPC method. In practice, if a method has any `params`, they are almost always of type `Array<any>`.

If the request fails for any reason, the `Promise` will reject with an Ethereum RPC Error.

OneKey Browser Extension supports most standardized Ethereum RPC methods, in addition to a number of methods that may not be supported by other wallets.

See the OneKey Browser Extension RPC API documentation for details.

**Example**

```javascript
const params = [  
    {    
        from: '0xb60e8dd61c5d32be8058bb8eb970870f0java7233155',    
        to: '0xd46e8dd67c5d32be8058bb8eb970870f07244567',    
        gas: '0x76c0', // 30400    
        gasPrice: '0x9184e72a000', // 10000000000000    
        value: '0x9184e72a', // 2441406250    
        data: '0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675',  
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

### Events

The OneKey Browser Extension provider implements the [Node.js `EventEmitter` API](https://nodejs.org/api/events.html#events_class_eventemitter). This section details the events emitted via that API.

There are innumerable `EventEmitter` guides elsewhere, but you can listen for events like this:

```javascript
window.$onekey.ethereum.on('accountsChanged', (accounts) => {
  // Handle the new accounts, or lack thereof.
  // "accounts" will always be an array, but it can be empty.
});

window.$onekey.ethereum.on('chainChanged', (chainId) => {
  // Handle the new chain.
  // Correctly handling chain changes can be complicated.
  // We recommend reloading the page unless you have good reason not to.
  window.location.reload();
});
```

#### `connect`

```typescript
interface ConnectInfo {
  chainId: string;
}

window.$onekey.ethereum.on('connect', handler: (connectInfo: ConnectInfo) => void);
```

The OneKey Browser Extension provider emits this event when it first becomes able to submit RPC requests to a chain.

We recommend using a `connect` event handler and the `window.$onekey.ethereum.isConnected()` method in order to determine when/if the provider is connected.

#### `disconnect`

```typescript
window.$onekey.ethereum.on('disconnect', handler: (error: ProviderRpcError) => void);
```

The OneKey provider emits this event if it becomes unable to submit RPC requests to any chain. In general, this will only happen due to network connectivity issues or some unforeseen error.

Once `disconnect` has been emitted, the provider will not accept any new requests until the connection to the chain has been re-established, which requires reloading the page. You can also use the `window.$onekey.ethereum.isConnected()` method to determine if the provider is disconnected.

#### `accountsChanged`

```typescript
window.$onekey.ethereum.on('accountsChanged', handler: (accounts: Array<string>) => void);
```

The OneKey provider emits this event whenever the return value of the `eth_accounts` RPC method changes. `eth_accounts` returns an array that is either empty or contains a single account address. The returned address, if any, is the address of the most recently used account that the caller is permitted to access. Callers are identified by their URL origin, which means that all sites with the same origin share the same permissions.

This means that `accountsChanged` will be emitted whenever the user's exposed account address changes.

> **Tip**\
> We plan to allow the `eth_accounts` array to be able to contain multiple addresses in the near future.

#### `chainChanged`

> **Tip**\
> See the Chain IDs section for OneKey Browser Extension's default chains and their chain IDs.

```typescript
window.$onekey.ethereum.on('chainChanged', handler: (chainId: string) => void);
```

The OneKey Browser Extension provider emits this event when the currently connected chain changes.

All RPC requests are submitted to the currently connected chain. Therefore, it's critical to keep track of the current chain ID by listening for this event.

A common way to handle this is to reload the page:

```javascript
window.$onekey.ethereum.on('chainChanged', (_chainId) => window.location.reload());
```

#### `message`

```typescript
interface ProviderMessage {
  type: string;
  data: unknown;
}

window.$onekey.ethereum.on('message', handler: (message: ProviderMessage) => void);
```

The OneKey provider emits this event when it receives some message that the consumer should be notified of. The kind of message is identified by the `type` string.

RPC subscription updates are a common use case for the `message` event. For example, if you create a subscription using `eth_subscribe`, each subscription update will be emitted as a `message` event with a type of `eth_subscription`.

### Errors

All errors thrown or returned by the OneKey provider follow this interface:

```typescript
interface ProviderRpcError extends Error {
  message: string;
  code: number;
  data?: unknown;
}
```

The `window.$onekey.ethereum.request(args)` method throws errors eagerly. You can often use the `code` property to determine why the request failed.

Common codes and their meaning include:

* **4001**: The request was rejected by the user.
* **-32602**: The parameters were invalid.
* **-32603**: Internal error.

For the complete list of errors, please see [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193#provider-errors) and [EIP-1474](https://eips.ethereum.org/EIPS/eip-1474).
