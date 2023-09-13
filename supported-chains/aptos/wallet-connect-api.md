# Wallet Connect API

## Basic Usage <a href="#basic-usage" id="basic-usage"></a>

First check out WalletConnect [documentation](https://docs.walletconnect.com/mobile-linking#wallet-support) for connecting wallet.

In addition to the `WalletConnect URI`, add a new parameter "network" to pass Aptos network information.

{% code overflow="wrap" %}
```
# Example
wc:00e46b69-d0cc-4b3e-b6a2-cee442f97188@1?bridge=https%3A%2F%2Fbridge.walletconnect.org&key=91303dedf64285cbbaf9120f6e9d160a5c8aa3deb67017a3874cd272323f48ae&network=aptos
```
{% endcode %}

If you have a OneKey wallet installed, it will connect to OneKey's current Aptos wallet.



> For more questions about connecting with WalletConnect, please refer to the [documentation](https://docs.walletconnect.com/client-api)

## Methods <a href="#ethereum-json-rpc-methods" id="ethereum-json-rpc-methods"></a>

> The following methods are all custom methods outside of WalletConnect, so you need to use [sendCustomRequest](https://docs.walletconnect.com/client-api#send-custom-request) to request them.

### account

**Tip**

If you haven't connected your wallet or haven't given your consent, this method won't get any information.

**Request**

```
walletConnector.sendCustomRequest({ method: "account" })
```

**Response**

{% code overflow="wrap" %}
```json
{
    "publicKey": "0xf7768b9884081c0a781b3bff6dd896acfdc2728a29fbe9b711ee4f39040d5afb",
    "address":"0xc046a096af4602ef60766ffd88033d178e5658cf31827049f3709c62a28a2680"
} 
```
{% endcode %}



### **network**

**Request**

```
walletConnector.sendCustomRequest({ method: "network" })
```

**Response**

{% code overflow="wrap" %}
```
"Mainnet" # Or "Testnet"
```
{% endcode %}



### **getChainId**

**Request**

```
walletConnector.sendCustomRequest({ method: "getChainId" })
```

**Response**

{% code overflow="wrap" %}
```json
{ "chainId": 1 } 
```
{% endcode %}



### signAndSubmitTransaction

**Request**\
Example Transaction, following an [EntryFunctionPayload](https://github.com/aptos-labs/aptos-core/blob/main/ecosystem/typescript/sdk/src/generated/models/EntryFunctionPayload.ts#L8-L21)

{% code overflow="wrap" %}
```
const transaction = {
    arguments: [address, '10000'],
    function: '0x1::coin::transfer',
    type: 'entry_function_payload',
    type_arguments: ['0x1::aptos_coin::AptosCoin'],
};

walletConnector.sendCustomRequest({ 
    method: "signAndSubmitTransaction",
    params: [transaction]
}})
```
{% endcode %}

**Response**

```
{
    "hash": "0xdcf7548f1c8679d1607ea7e5102cc75f19af355058c0ffe4ef9e5703f133adc7",
    "sender": "0xd7663c457cfb8516f400454c4d31b43a0161596d5109e3b789848f371fbaee8d",
    "sequence_number": "16",
    "max_gas_amount": "890",
    "gas_unit_price": "100",
    "expiration_timestamp_secs": "1666586889",
    "payload": {
        "function": "0x1::coin::transfer",
        "type_arguments": [
        "0x1::aptos_coin::AptosCoin"
        ],
        "arguments": [
        "0xd7663c457cfb8516f400454c4d31b43a0161596d5109e3b789848f371fbaee8d",
        "100000"
        ],
        "type": "entry_function_payload"
    },
    "signature": {
        "public_key": "0xc35b5938c4d1524641e5732430bd101a22dc9551d8e554366aa61e07f497a4fb",
        "signature": "0x901a2c404e7d6f6427228bc0f1498217ac2429c198583f2a693390d5d28f5cc6313505886accb8673eb1e80e9c85ca91adc13dc32eb7542a50c3318fc3e6f906",
        "type": "ed25519_signature"    
    },
}
```

follow [PendingTransaction](https://github.com/aptos-labs/aptos-core/blob/30b385bf38/ecosystem/typescript/sdk/src/generated/models/PendingTransaction.ts#L14)



### signMessage

**Request**

```
export interface SignMessagePayload {
  address?: boolean; // Should we include the address of the account in the message
  application?: boolean; // Should we include the domain of the dapp
  chainId?: boolean; // Should we include the current chain id the wallet is connected to
  message: string; // The message to be signed and displayed to the user
  nonce: string; // A nonce the dapp should generate
}

const signMessagePayload = {
    address: false,
    application: true,
    chainId: true,
    message: "This is a sample message",
    nonce: 12345,
}

walletConnector.sendCustomRequest({ 
    method: "signMessage",
    params: [signMessagePayload]
}})
```

**Response**

{% code overflow="wrap" %}
```json
export interface SignMessageResponse {
  address: string;
  application: string;
  chainId: number;
  fullMessage: string; // The message that was generated to sign
  message: string; // The message passed in by the user
  nonce: string,
  prefix: string, // Should always be APTOS
  signature: string; // The signed full message
}

{
    "message": "This is a sample message",
    "nonce": 12345,
    "prefix": "APTOS",
    "signature": "0x8995a00d31ff10f4d6d7cc4a8485fbe8aa04fb265b0f0629cf0184b9d37925e4bda001928a59f5c287f07dca9d9147b2b2ba004633cd4980ae0839fa1b34b70a",
    "fullMessage": "APTOS\napplication: localhost:3000\nchainId: 1\nmessage: This is a sample message\nnonce: 12345",
    "application": "http://localhost:3000",
    "chainId": 1
}
```
{% endcode %}



## **Event**

**Todo**



## Example

[Onekey Aptos WalletConnect Example](https://github.com/OneKeyHQ/cross-inpage-provider/blob/master/packages/example/components/aptosWalletConnect/AptosExample.tsx)



