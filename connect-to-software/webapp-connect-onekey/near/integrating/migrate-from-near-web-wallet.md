# Migrate from Near Web Wallet

Although, we strongly recommend using formal API to interact your applicactions with OneKey Wallet, we still provide some legacy API that is very similar to [Near Web Wallet](https://wallet.near.org/), make it easier for you migrating existing codes from Near Web Wallet.

```javascript
import * as nearAPI from "near-api-js";

const config = {
  networkId: 'mainnet',
  nodeUrl: 'https://rpc.mainnet.near.org',
  // networkId: 'testnet',
  // nodeUrl: 'https://rpc.testnet.near.org',
  headers: {},
  keyStore: new nearAPI.keyStores.BrowserLocalStorageKeyStore(),
}
const near = new nearAPI.Near(config);
const connection = near.connection;

const provider = new OneKeyNearProvider({
  connection,
  networkId: 'mainnet', // `mainnet` or `testnet`
  // reloading page after signIn or signTransaction that likes web wallet callback url working
  enablePageReload: true, 
});

provider.requestSignIn({
  successUrl,
  failureUrl,
  contractId?,
});

provider.getAccountId();

provider.isSignedIn();

provider.signOut();

provider.requestSignTransactions({
  transactions: [],
  callbackUrl,
  meta?,
});

provider.account().signAndSendTransaction({
  receiverId,
  actions: [],
  walletCallbackUrl,
  walletMeta?,
});

```

{% hint style="info" %}
Legacy API is not well tested and full case coveraged.  Try to use formal API as possible as you can.&#x20;
{% endhint %}
