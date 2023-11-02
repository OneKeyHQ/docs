# Introduction

Welcome to OneKey’s Developer Documentation. This documentation is for learning to develop applications for OneKey.

* You can find the latest version of OneKey on our [Official Website](https://onekey.so).
* For help using OneKey, visit our [User Support Site](https://help.onekey.so/).
* For up to the minute news, follow our [Twitter](https://twitter.com/OneKeyHQ).
* To learn how to contribute to the OneKey project itself, visit our [Github Repos](https://github.com/OneKeyHQ/app-monorepo).

### Why OneKey&#x20;

OneKey is the smartest way to secure, buy, exchange and grow your crypto assets.&#x20;

It is created to meet the needs of secure and usable Ethereum and other chains (e.g. Bitcoin, Solana, Aptos, Starcoin...) based dApps. In particular, it handles account management and connecting the user to the blockchain.

* [Get started here](guide/getting-started.md)
* Learn more about our [JavaScript Provider API](connect-to-software/dapp-connect-to-onekey/eth/provider-api.md)
* Learn more about our [RPC API](connect-to-software/dapp-connect-to-onekey/eth/rpc-api.md)



### Account Management

OneKey Browser Extension allows users to manage accounts and their keys in a variety of ways, including hardware wallets, while isolating them from the site context. This is a great security improvement over storing the user keys on a single central server, or even in local storage, which can allow for [mass account thefts](https://www.ccn.com/cryptocurrency-exchange-etherdelta-hacked-in-dns-hijacking-scheme/).

This security feature also comes with developer convenience: For developers, you simply interact with the globally available `ethereum` API that identifies the users of web3-compatible browsers (like OneKey users), and whenever you request a transaction signature (like `eth_sendTransaction`, `eth_signTypedData`, or others), OneKey will prompt the user in as comprehensible a way as possible.

###

### Blockchain Connection

OneKey comes pre-loaded with fast connections to the Ethereum blockchain and several other networks. This allows you to get started without synchronizing a full node, while still providing the option to upgrade your security and use the blockchain provider of your choice.

Today, OneKey is compatible with any blockchain that exposes an [Ethereum-compatible JSON RPC API](https://eth.wiki/json-rpc/API), including custom and private blockchains. For development, we recommend running a test blockchain like [Ganache](https://www.trufflesuite.com/ganache).

We’re aware that there are constantly new private blockchains that people are interested in connecting OneKey to, and we are building towards easier integration with these many options.
