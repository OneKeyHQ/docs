# ethereumSignTransaction

import Playground from "@src/components/playground";

### Ethereum: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

ES6

```javascript
const result = await OneKeyConnect.ethereumSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.ethereumSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L69-L72)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `Object` type of [EthereumTransaction](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/ethereum.js#L5) "0x" prefix for each field is optional

#### Example

```javascript
OneKeyConnect.ethereumSignTransaction({
    path: "m/44'/60'/0'",
    transaction: {
        to: "0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8",
        value: "0xf4240",
        data: "0x01",
        chainId: 1,
        nonce: "0x0",
        gasLimit: "0x5208",
        gasPrice: "0xbebc200"
    }
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L57-L60)

```javascript
{
    success: true,
    payload: {
        v: string, // hexadecimal string with "0x" prefix
        r: string, // hexadecimal string with "0x" prefix
        s: string, // hexadecimal string with "0x" prefix
    }
}
```

Error

```javascript
{
    success: false,
    payload: {
        error: string // error message
    }
}
```

\<Playground initValue={ `OneKeyConnect.ethereumSignTransaction({ path: "m/44'/60'/0'", transaction: { to: "0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8", value: "0xf4240", data: "0x01", chainId: 1, nonce: "0x0", gasLimit: "0x5208", gasPrice: "0xbebc200" } });`} />
