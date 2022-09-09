# composeTransaction

import Playground from "@src/components/playground";

### Compose transaction

Requests a payment from the users wallet to a set of given outputs. Internally a BIP-0044 account discovery is performed and user is presented with a list of accounts. After account selection user is presented with list of fee selection. After selecting a fee transaction is signed and returned in hexadecimal format. Change output is added automatically, if needed.

ES6

```javascript
const result = await OneKeyConnect.composeTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.composeTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

* `outputs` — _required_ `Array` of recipients Objects described below
* `coin` — _required_ `string` determines network definition specified in [coins.json](https://github.com/OneKeyHQ/connect/blob/onekey/src/data/coins.json) file. Coin `shortcut`, `name` or `label` can be used.
* `push` — _optional_ `boolean` determines if composed transaction will be broadcasted into blockchain.

#### Outputs objects:

* `regular output`
  * `amount` - _required_ `string` value to send in satohosi
  * `address` - _required_ `string` recipient address
* `send-max` - spends all available inputs from account
  * `type` - _required_ with `send-max` value
  * `address` - _required_ `string` recipient address
* `opreturn` - [read more](https://en.bitcoin.it/wiki/OP\_RETURN)
  * `type` - _required_ with `opreturn` value
  * `dataHex` - _optional_ `hexadecimal string` with arbitrary data

#### Example

Send 0.002 BTC to "18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2"

```javascript
OneKeyConnect.composeTransaction({
    outputs: [
        { amount: "200000", address: "18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2" }
    ],
    coin: "btc",
    push: true
});
```

#### Result

```javascript
{
    success: true,
    payload: {
        signatures: Array<string>, // signer signatures
        serializedTx: string,        // serialized transaction
        txid?: string,             // blockchain transaction id
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

\<Playground initValue={ `OneKeyConnect.composeTransaction({ outputs: [ { amount: "200000", address: "18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2" } ], coin: "btc", push: true });`} />
