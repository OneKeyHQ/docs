# Connect a dApp

Use a wallet provider or a wallet connection library to connect your dApp to OneKey. The wallet handles account access and presents signing requests to the user.

For the current guided integration, start with the [developer portal quickstart](https://developer.onekey.so/en/connect-to-software/).

## Before you start

* Install [OneKey](https://onekey.so/download/) in your development environment.
* Run provider detection in browser code, not during server rendering.
* Choose the chain you need. Provider methods and return values are chain-specific.

## Choose a connection method

| Method            | Use it when                                                   | Guide                                                               |
| ----------------- | ------------------------------------------------------------- | ------------------------------------------------------------------- |
| Injected provider | Your dApp runs in a browser with OneKey available             | [Provider reference](../connect-to-software/webapp-connect-onekey/) |
| Wallet UI library | You want a wallet chooser or already use a supported library  | [Wallet UI libraries](../connect-to-software/support-wallet-kit/)   |
| WalletConnect     | Your app needs a WalletConnect session with the OneKey wallet | [WalletConnect](../connect-to-software/using-walletconnect/)        |

For Ethereum, OneKey exposes an EIP-1193-style provider. Read [Ethereum provider API](../connect-to-software/webapp-connect-onekey/eth/provider-api.md) and [account access](../connect-to-software/webapp-connect-onekey/eth/accessing-accounts.md) before adding transaction requests.

## Handle the complete connection flow

1. Detect the provider and show a clear install or connection option if it is missing.
2. Request account access after an explicit user action.
3. Display the selected account and network.
4. Update application state when the account or network changes.
5. Handle rejected requests without repeatedly prompting the user.

A wallet connection is not permission to sign every future request. Keep signing and transaction submission explicit in your application UI.

## Next steps

* [Sending Ethereum transactions](../connect-to-software/webapp-connect-onekey/eth/sending-transactions.md)
* [Signing data](../connect-to-software/webapp-connect-onekey/eth/signing-data.md)
* [Mobile and deep links](https://developer.onekey.so/en/connect-to-software/mobile-deeplinks/)
* [Troubleshooting](../trouble-shooting.md)
