# liskSignTransaction

import Playground from "@src/components/playground";

### Lisk: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

ES6

```javascript
const result = await OneKeyConnect.liskSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.liskSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#L121-L124)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `Object` type of [LiskTransaction](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#42-L52)

#### Example

```javascript
OneKeyConnect.liskSignTransaction(
    path: "m/44'/134'/0'/0'",
    transaction: {
        amount: "10000000",
        recipientId: "9971262264659915921L",
        timestamp: 57525937,
        type: 0,
        fee: "20000000",
        asset: {
            data: "Test data"
        }
    }
);
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#L126-L129)

```javascript
{
    success: true,
    payload: {
        signature: string
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

\<Playground initValue={ `OneKeyConnect.liskSignTransaction({ path: "m/44'/134'/0'/0'", transaction: { amount: "10000000", recipientId: "9971262264659915921L", timestamp: 57525937, type: 0, fee: "20000000", asset: { data: "Test data" } } } );`} />
