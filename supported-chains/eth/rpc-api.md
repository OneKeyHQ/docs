# RPC API

## RPC API

OneKey uses the `onekey.request(args)` method to wrap an RPC API.

The API is based on an interface exposed by all Ethereum clients, along with a growing number of methods that may or may not be supported by other wallets.



**Tip**

All RPC method requests can return errors. Make sure to handle errors for every call to `onekey.request(args)`.

### Ethereum JSON-RPC Methods[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#ethereum-json-rpc-methods) <a href="#ethereum-json-rpc-methods" id="ethereum-json-rpc-methods"></a>

For the Ethereum JSON-RPC API, please see [the Ethereum wiki](https://eth.wiki/json-rpc/API#json-rpc-methods).

Important methods from this API include:

* [`eth_accounts`](https://eth.wiki/json-rpc/API#eth\_accounts)
* [`eth_call`](https://eth.wiki/json-rpc/API#eth\_call)
* [`eth_getBalance`](https://eth.wiki/json-rpc/API#eth\_getbalance)
* [`eth_sendTransaction`](https://eth.wiki/json-rpc/API#eth\_sendtransaction)
* [`eth_sign`](https://eth.wiki/json-rpc/API#eth\_sign)

### Permissions[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#permissions) <a href="#permissions" id="permissions"></a>

OneKey introduced Web3 Wallet Permissions via [EIP-2255](https://eips.ethereum.org/EIPS/eip-2255). In this permissions system, each RPC method is either _restricted_ or _open_. If a method is restricted, an external _domain_ (like a web3 site) must have the corresponding permission in order to call it. Open methods, meanwhile, do not require permissions to call, but may require confirmation by the user in order to succeed (e.g. `eth_sendTransaction`).

Currently, the only permission is `eth_accounts`, which allows you to access the user's Ethereum address(es). More permissions will be added in the future.

Under the hood, permissions are plain, JSON-compatible objects, with a number of fields that are mostly used internally by OneKey. The following interface lists the fields that may be of interest to consumers:

```
interface Web3WalletPermission {  // The name of the method corresponding to the permission  parentCapability: string;
  // The date the permission was granted, in UNIX epoch time  date?: number;}
```

The permissions system is implemented in the [`rpc-cap` package](https://github.com/onekeyhq/rpc-cap). If you're interested in learning more about the theory behind this _capability_-inspired permissions system, we encourage you to take a look at [EIP-2255](https://eips.ethereum.org/EIPS/eip-2255).

#### eth\_requestAccounts[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#eth\_requestaccounts) <a href="#eth_requestaccounts" id="eth_requestaccounts"></a>



**EIP-1102**

This method is specified by [EIP-1102](https://eips.ethereum.org/EIPS/eip-1102). It is equivalent to the deprecated `onekey.enable()` provider API method.

Under the hood, it calls [`wallet_requestPermissions`](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#wallet-requestpermissions) for the `eth_accounts` permission. Since `eth_accounts` is currently the only permission, this method is all you need for now.

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns)

`string[]` - An array of a single, hexadecimal Ethereum address string.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description)

Requests that the user provides an Ethereum address to be identified by. Returns a Promise that resolves to an array of a single Ethereum address string. If the user denies the request, the Promise will reject with a `4001` error.

The request causes a OneKey popup to appear. You should only request the user's accounts in response to user action, such as a button click. You should always disable the button that caused the request to be dispatched, while the request is still pending.

If you can't retrieve the user's account(s), you should encourage the user to initiate an account request.

**Example**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#example)

```
document.getElementById('connectButton', connect);
function connect() {  onekey    .request({ method: 'eth_requestAccounts' })    .then(handleAccountsChanged)    .catch((error) => {      if (error.code === 4001) {        // EIP-1193 userRejectedRequest error        console.log('Please connect to OneKey.');      } else {        console.error(error);      }    });}
```

#### wallet\_getPermissions[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#wallet\_getpermissions) <a href="#wallet_getpermissions" id="wallet_getpermissions"></a>



**Platform Availability**

This RPC method is not yet available in OneKey Mobile.

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns-1)

`Web3WalletPermission[]` - An array of the caller's permissions.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description-1)

Gets the caller's current permissions. Returns a Promise that resolves to an array of `Web3WalletPermission` objects. If the caller has no permissions, the array will be empty.

#### wallet\_requestPermissions[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#wallet\_requestpermissions) <a href="#wallet_requestpermissions" id="wallet_requestpermissions"></a>



**Platform Availability**

This RPC method is not yet available in OneKey Mobile.

**Parameters**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#parameters)

* `Array`
  1. `RequestedPermissions` - The requested permissions.

```
interface RequestedPermissions {  [methodName: string]: {}; // an empty object, for future extensibility}
```

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns-2)

`Web3WalletPermission[]` - An array of the caller's permissions.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description-2)

Requests the given permissions from the user. Returns a Promise that resolves to a non-empty array of `Web3WalletPermission` objects, corresponding to the caller's current permissions. If the user denies the request, the Promise will reject with a `4001` error.

The request causes a OneKey popup to appear. You should only request permissions in response to user action, such as a button click.

**Example**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#example-1)

```
document.getElementById('requestPermissionsButton', requestPermissions);
function requestPermissions() {  onekey    .request({      method: 'wallet_requestPermissions',      params: [{ eth_accounts: {} }],    })    .then((permissions) => {      const accountsPermission = permissions.find(        (permission) => permission.parentCapability === 'eth_accounts'      );      if (accountsPermission) {        console.log('eth_accounts permission successfully requested!');      }    })    .catch((error) => {      if (error.code === 4001) {        // EIP-1193 userRejectedRequest error        console.log('Permissions needed to continue.');      } else {        console.error(error);      }    });}
```

### Other RPC Methods[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#other-rpc-methods) <a href="#other-rpc-methods" id="other-rpc-methods"></a>

#### eth\_decrypt[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#eth\_decrypt) <a href="#eth_decrypt" id="eth_decrypt"></a>



**Platform Availability**

This RPC method is not yet available in OneKey Mobile.

**Parameters**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#parameters-1)

* `Array`
  1. `string` - An encrypted message.
  2. `string` - The address of the Ethereum account that can decrypt the message.

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns-3)

`string` - The decrypted message.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description-3)

Requests that OneKey decrypts the given encrypted message. The message must have been encrypted using the public encryption key of the given Ethereum address. Returns a Promise that resolves to the decrypted message, or rejects if the decryption attempt fails.

See [`eth_getEncryptionPublicKey`](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#eth-getencryptionpublickey) for more information.

**Example**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#example-2)

```
onekey  .request({    method: 'eth_decrypt',    params: [encryptedMessage, accounts[0]],  })  .then((decryptedMessage) =>    console.log('The decrypted message is:', decryptedMessage)  )  .catch((error) => console.log(error.message));
```

#### eth\_getEncryptionPublicKey[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#eth\_getencryptionpublickey) <a href="#eth_getencryptionpublickey" id="eth_getencryptionpublickey"></a>



**Platform Availability**

This RPC method is not yet available in OneKey Mobile.

**Parameters**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#parameters-2)

* `Array`
  1. `string` - The address of the Ethereum account whose encryption key should be retrieved.

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns-4)

`string` - The public encryption key of the specified Ethereum account.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description-4)

Requests that the user shares their public encryption key. Returns a Promise that resolve to the public encryption key, or rejects if the user denied the request.

The public key is computed from entropy associated with the specified user account, using the [`nacl`](https://github.com/dchest/tweetnacl-js) implementation of the `X25519_XSalsa20_Poly1305` algorithm.

**Example**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#example-3)

```
let encryptionPublicKey;
onekey  .request({    method: 'eth_getEncryptionPublicKey',    params: [accounts[0]], // you must have access to the specified account  })  .then((result) => {    encryptionPublicKey = result;  })  .catch((error) => {    if (error.code === 4001) {      // EIP-1193 userRejectedRequest error      console.log("We can't encrypt anything without the key.");    } else {      console.error(error);    }  });
```

**Encrypting**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#encrypting)

The point of the encryption key is of course to encrypt things. Here's an example of how to encrypt a message using [`eth-sig-util`](https://github.com/OneKeyhq/eth-sig-util):

```
const ethUtil = require('ethereumjs-util');
const encryptedMessage = ethUtil.bufferToHex(  Buffer.from(    JSON.stringify(      sigUtil.encrypt(        encryptionPublicKey,        { data: 'Hello world!' },        'x25519-xsalsa20-poly1305'      )    ),    'utf8'  ));
```

#### wallet\_addEthereumChain[#](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#wallet\_addethereumchain) <a href="#wallet_addethereumchain" id="wallet_addethereumchain"></a>



**EIP-3085**

This method is specified by [EIP-3085](https://eips.ethereum.org/EIPS/eip-3085).

**Parameters**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#parameters-3)

* `Array`
  1. `AddEthereumChainParameter` - Metadata about the chain that will be added to OneKey.

For the `rpcUrls` and `blockExplorerUrls` arrays, at least one element is required, and only the first element will be used.

```
interface AddEthereumChainParameter {  chainId: string; // A 0x-prefixed hexadecimal string  chainName: string;  nativeCurrency: {    name: string;    symbol: string; // 2-6 characters long    decimals: 18;  };  rpcUrls: string[];  blockExplorerUrls?: string[];  iconUrls?: string[]; // Currently ignored.}
```

**Returns**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#returns-5)

`null` - The method returns `null` if the request was successful, and an error otherwise.

**Description**[**#**](https://docs.onekey.so/en/Extension/API%20Reference/rpc-api#description-5)

Creates a confirmation asking the user to add the specified chain to OneKey. The user may choose to switch to the chain once it has been added.

As with any method that causes a confirmation to appear, `wallet_addEthereumChain` should **only** be called as a result of direct user action, such as the click of a button.

OneKey stringently validates the parameters for this method, and will reject the request if any parameter is incorrectly formatted. In addition, OneKey will reject the request under the following circumstances:

* If the RPC endpoint doesn't respond to RPC calls.
* If the RPC endpoint returns a different chain ID when `eth_chainId` is called.
* If the chain ID corresponds to any default OneKey chains.

OneKey does not yet support chains with native currencies that do not have 18 decimals, but may do so in the future.
