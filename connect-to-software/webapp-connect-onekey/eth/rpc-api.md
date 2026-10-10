# RPC API

OneKey uses the `window.$onekey.ethereum.request(args)` method to wrap an RPC API.

The API is based on an interface exposed by all Ethereum clients, along with a growing number of methods that may or may not be supported by other wallets.

**Tip**

All RPC method requests can return errors. Make sure to handle errors for every call to `window.$onekey.ethereum.request(args)`.

## Ethereum JSON-RPC Methods <a href="#ethereum-json-rpc-methods" id="ethereum-json-rpc-methods"></a>

For the Ethereum JSON-RPC API, please see [the Ethereum wiki](https://ethereum.org/en/developers/docs/apis/json-rpc/#json-rpc-methods).

Important methods from this API include:

* [`eth_accounts`](https://ethereum.org/en/developers/docs/apis/json-rpc/#eth_accounts)
* [`eth_call`](https://ethereum.org/en/developers/docs/apis/json-rpc/#eth_call)
* [`eth_getBalance`](https://ethereum.org/en/developers/docs/apis/json-rpc/#eth_getbalance)
* [`eth_sendTransaction`](https://ethereum.org/en/developers/docs/apis/json-rpc/#eth_sendtransaction)
* [`eth_sign`](https://ethereum.org/en/developers/docs/apis/json-rpc/#eth_sign)

## Permissions <a href="#permissions" id="permissions"></a>

OneKey introduced Web3 Wallet Permissions via [EIP-2255](https://eips.ethereum.org/EIPS/eip-2255). In this permissions system, each RPC method is either _restricted_ or _open_. If a method is restricted, an external _domain_ (like a web3 site) must have the corresponding permission in order to call it. Open methods, meanwhile, do not require permissions to call, but may require confirmation by the user in order to succeed (e.g. `eth_sendTransaction`).

Currently, the only permission is `eth_accounts`, which allows you to access the user's Ethereum address(es). Check method availability in the target wallet version.

Under the hood, permissions are plain, JSON-compatible objects, with a number of fields that are mostly used internally by OneKey. The following interface lists the fields that may be of interest to consumers:

```typescript
interface Web3WalletPermission {
  // The name of the method corresponding to the permission
  parentCapability: string;
  // The date the permission was granted, in UNIX epoch time
  date?: number;
}
```

If you're interested in learning more about the theory behind this _capability_-inspired permissions system, we encourage you to take a look at [EIP-2255](https://eips.ethereum.org/EIPS/eip-2255).

### eth\_requestAccounts <a href="#eth_requestaccounts" id="eth_requestaccounts"></a>

**EIP-1102**

This method is specified by [EIP-1102](https://eips.ethereum.org/EIPS/eip-1102). It is equivalent to the deprecated `onekey.enable()` provider API method.

Use this method to request account access. Advanced permission requests are described under [wallet\_requestPermissions](rpc-api.md#wallet_requestpermissions).

**Returns**

`string[]` - An array of a single, hexadecimal Ethereum address string.

**Description**

Requests that the user provides an Ethereum address to be identified by. Returns a Promise that resolvess to an array of a single Ethereum address string. If the user denies the request, the Promise will reject with a `4001` error.

The request causes a OneKey popup to appear. You should only request the user's accounts in response to user action, such as a button click. You should always disable the button that caused the request to be dispatched, while the request is still pending.

If you can't retrieve the user's account(s), you should encourage the user to initiate an account request.

**Example**

```javascript
document.getElementById('connectButton')?.addEventListener('click', connect);
function connect() {
    window.$onekey.ethereum.request({ method: 'eth_requestAccounts' })
        .then(handleAccountsChanged)
        .catch((error) => {
            if (error.code === 4001) {
                // EIP-1193 userRejectedRequest error
                console.log('Please connect to OneKey.');
            } else {
                console.error(error);
            }
        });
}
```

### wallet\_getPermissions <a href="#wallet_getpermissions" id="wallet_getpermissions"></a>

**Platform Availability**

Availability depends on the wallet platform and version. Handle unsupported-method errors; the injected provider forwarding a request does not guarantee backend support.

**Returns**

`Web3WalletPermission[]` - An array of the caller's permissions.

**Description**

Gets the caller's current permissions. Returns a Promise that resolvess to an array of `Web3WalletPermission` objects. If the caller has no permissions, the array will be empty.

### wallet\_requestPermissions <a href="#wallet_requestpermissions" id="wallet_requestpermissions"></a>

**Platform Availability**

Availability depends on the wallet platform and version. Handle unsupported-method errors; the injected provider forwarding a request does not guarantee backend support.

**Parameters**

* `Array`
  1. `RequestedPermissions` - The requested permissions.

```typescript
interface RequestedPermissions {
  [methodName: string]: Record<string, never>;
}
```

**Returns**

`Web3WalletPermission[]` - An array of the caller's permissions.

**Description**

Requests the given permissions from the user. Returns a Promise that resolvess to a non-empty array of `Web3WalletPermission` objects, corresponding to the caller's current permissions. If the user denies the request, the Promise will reject with a `4001` error.

The request causes a OneKey popup to appear. You should only request permissions in response to user action, such as a button click.

**Example**

```javascript
document.getElementById('requestPermissionsButton')?.addEventListener('click', requestPermissions);
function requestPermissions() {
    window.$onekey.ethereum.request({
        method: 'wallet_requestPermissions',
        params: [{ eth_accounts: {} }],
    })
    .then((permissions) => {
        const accountsPermission = permissions.find(
            (permission) => permission.parentCapability === 'eth_accounts'
        );
        if (accountsPermission) {
            console.log('eth_accounts permission successfully requested!');
        }
    })
    .catch((error) => {
        if (error.code === 4001) {
            // EIP-1193 userRejectedRequest error
            console.log('Permissions needed to continue.');
        } else {
            console.error(error);
        }
    });
}
```

## Other RPC Methods <a href="#other-rpc-methods" id="other-rpc-methods"></a>

### eth\_decrypt <a href="#eth_decrypt" id="eth_decrypt"></a>

**Platform Availability**

Availability depends on the wallet platform and version. Handle unsupported-method errors; the injected provider forwarding a request does not guarantee backend support.

**Parameters**

* `Array`
  1. `string` - An encrypted message.
  2. `string` - The address of the Ethereum account that can decrypt the message.

**Returns**

`string` - The decrypted message.

**Description**

Requests that OneKey decrypts the given encrypted message. The message must have been encrypted using the public encryption key of the given Ethereum address. Returns a Promise that resolvess to the decrypted message, or rejects if the decryption attempt fails.

See [`eth_getEncryptionPublicKey`](rpc-api.md#eth_getencryptionpublickey) for more information.

**Example**

```javascript
window.$onekey.ethereum.request({
    method: 'eth_decrypt',
    params: [encryptedMessage, accounts[0]],
})
.then((decryptedMessage) =>    console.log('The decrypted message is:', decryptedMessage)  )
.catch((error) => console.log(error.message));
```

### eth\_getEncryptionPublicKey <a href="#eth_getencryptionpublickey" id="eth_getencryptionpublickey"></a>

**Platform Availability**

Availability depends on the wallet platform and version. Handle unsupported-method errors; the injected provider forwarding a request does not guarantee backend support.

**Parameters**

* `Array`
  1. `string` - The address of the Ethereum account whose encryption key should be retrieved.

**Returns**

`string` - The public encryption key of the specified Ethereum account.

**Description**

Requests that the user shares their public encryption key. Returns a Promise that resolves to the public encryption key, or rejects if the user denied the request.

The public key is computed from entropy associated with the specified user account, using the [`nacl`](https://github.com/dchest/tweetnacl-js) implementation of the `X25519_XSalsa20_Poly1305` algorithm.

**Example**

```javascript
let encryptionPublicKey;
window.$onekey.ethereum.request({
    method: 'eth_getEncryptionPublicKey',
    params: [accounts[0]],
    // you must have access to the specified account
})
.then((result) => {    encryptionPublicKey = result;  })
.catch((error) => {
    if (error.code === 4001) {
        // EIP-1193 userRejectedRequest error
        console.log("We can't encrypt anything without the key.");
    } else {
        console.error(error);
    }
});
```

**Encrypting**

The point of the encryption key is of course to encrypt things. Here's an example of how to encrypt a message using [`eth-sig-util`](https://github.com/OneKeyhq/eth-sig-util):

```javascript
// Legacy encryption helper API; confirm its signature in your installed version.
// `sigUtil` and `ethUtil` must be imported from the packages your app uses.
const encrypted = sigUtil.encrypt(
  encryptionPublicKey,
  { data: 'Hello world!' },
  'x25519-xsalsa20-poly1305',
);
const encryptedMessage = ethUtil.bufferToHex(
  Buffer.from(JSON.stringify(encrypted), 'utf8'),
);
```

### wallet\_addEthereumChain <a href="#wallet_addethereumchain" id="wallet_addethereumchain"></a>

**EIP-3085**

This method is specified by [EIP-3085](https://eips.ethereum.org/EIPS/eip-3085).

**Parameters**

* `Array`
  1. `AddEthereumChainParameter` - Metadata about the chain that will be added to OneKey.

Provide at least one valid RPC URL. `blockExplorerUrls` and `iconUrls` are optional metadata; acceptance and display depend on the wallet.

```typescript
interface AddEthereumChainParameter {
  chainId: string; // A 0x-prefixed hexadecimal chain ID.
  chainName: string;
  nativeCurrency: {
    name: string;
    symbol: string;
    decimals: number;
  };
  rpcUrls: string[];
  blockExplorerUrls?: string[];
  iconUrls?: string[];
}
```

**Returns**

`null` - The method returns `null` if the request was successful, and an error otherwise.

**Description**

Creates a confirmation asking the user to add the specified chain to OneKey. The user may choose to switch to the chain once it has been added.

As with any method that causes a confirmation to appear, `wallet_addEthereumChain` should **only** be called as a result of direct user action, such as the click of a button.

OneKey stringently validates the parameters for this method, and will reject the request if any parameter is incorrectly formatted. In addition, OneKey will reject the request under the following circumstances:

* If the RPC endpoint doesn't respond to RPC calls.
* If the RPC endpoint returns a different chain ID when `eth_chainId` is called.
* If the supplied metadata or endpoint is rejected by the wallet.

Check the current chain with `eth_chainId` after the request. Adding a chain does not guarantee that the wallet switched to it.
