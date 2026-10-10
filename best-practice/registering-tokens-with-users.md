# Suggest a Token to Users

Use `wallet_watchAsset` to ask the wallet to track a token. The user decides whether to add it. This request does not transfer tokens or establish that a token is trustworthy.

Confirm that the wallet is on the intended network, then call this method from a user action. Verify the contract address, symbol, and decimals before presenting the request.

```javascript
const provider = window.$onekey?.ethereum;
if (!provider) throw new Error('OneKey Ethereum provider is unavailable.');

try {
  const wasAdded = await provider.request({
    method: 'wallet_watchAsset',
    params: {
      type: 'ERC20',
      options: {
        address: '<TOKEN_CONTRACT_ADDRESS>',
        symbol: 'TOKEN',
        decimals: 18, // Replace with the token's actual decimals.
        image: 'https://example.com/token.png',
      },
    },
  });
  console.log(wasAdded ? 'Token added.' : 'Token was not added.');
} catch (error) {
  console.error(error.code, error.message);
}
```

Replace all example metadata before use. See [EIP-747](https://eips.ethereum.org/EIPS/eip-747) for the request format, and handle unsupported-method or user-rejection errors in your interface.
