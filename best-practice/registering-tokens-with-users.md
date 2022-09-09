# Registering Tokens with Users

When a user opens their OneKey Browser Extension, they are shown a variety of assets, including tokens. By default, OneKey Browser Extension auto-detects some major popular tokens and auto-displays them, but for most tokens, the user will need to add the token themselves.

While this is possible using our UI with the `Add Token` button, that process can be cumbersome, and involves the user interacting with contract addresses, and is very error prone.

You can greatly improve the security and experience of users adding your token to their OneKey Browser Extension by taking advantage of the `wallet_watchAsset` API as defined in [EIP-747](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-747.md).

### Example <a href="#example" id="example"></a>

If you'd like to integrate suggesting a token into your own web app, you can follow this code snippet to implement it:

```javascript
const tokenAddress = '0xd00981105e61274c8a5cd5a88fe7e037d935b513';const tokenSymbol = 'TUT';const tokenDecimals = 18;const tokenImage = 'http://placekitten.com/200/300';
try {  // wasAdded is a boolean. Like any RPC method, an error may be thrown.  const wasAdded = await onekey.request({    method: 'wallet_watchAsset',    params: {      type: 'ERC20', // Initially only supports ERC20, but eventually more!      options: {        address: tokenAddress, // The address that the token is at.        symbol: tokenSymbol, // A ticker symbol or shorthand, up to 5 chars.        decimals: tokenDecimals, // The number of decimals in the token        image: tokenImage, // A string url of the token logo      },    },  });
  if (wasAdded) {    console.log('Thanks for your interest!');  } else {    console.log('Your loss!');  }} catch (error) {  console.log(error);}
```
