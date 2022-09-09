# solanaSignTransaction

import Playground from "@src/components/playground";

### Solana: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

:::warning warning

* This method is supported after OneKey Connect JS-SDK version v8.6.0, and npm modules equal or larger than this version can be used.
* The OneKey device firmware may also be different for the user, caller should handle the response of method like firmware not support issue. :::

ES6

```javascript
const result = await OneKeyConnect.solanaSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.solanaSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ `string` base58 encoded string.

**Sign bundle of transaction**

* `bundle` - `Array` of Objects with `path` and `rawTx` fields

#### Example

Sign transaction by the first solana account:

```javascript
OneKeyConnect.solanaSignTransaction({
    path: "m/44'/501'/0'/0'",
    rawTx: 'QwE1XMGNhYCYcry9Y8tupaw8KBShus8uUNjsYjZyEZF7eezR8amPoNeT18uS69mkedxqugroVwJTqofA2jLbXtK9QB87vuUsK8VXYf2Ehvri4o9GSZKC9CZ7i5D8jBzJe7ZZtsySfy7BcB7yko3bhHWbpcVX9Unb',
});
```

Sign multiple transaction:

```javascript
OneKeyConnect.solanaSignTransaction({
    bundle: [
        {
            path: "m/44'/501'/0'/0'",
            rawTx: 'QwE1XMGNhYCYcry9Y8tupaw8KBShus8uUNjsYjZyEZF7eezR8amPoNeT18uS69mkedxqugroVwJTqofA2jLbXtK9QB87vuUsK8VXYf2Ehvri4o9GSZKC9CZ7i5D8jBzJe7ZZtsySfy7BcB7yko3bhHWbpcVX9Unb',
        },
        {
            path: "m/44'/501'/1'/0'",
            rawTx: 'QwE1XMGNhYCYcry9Y8tupaw8KBShus8uUNjsYjZyEZF7eezR8amPoNeT18uS69mkedxqugroVwJTqofA2jLbXtK9QB87vuUsK8VXYf2Ehvri4o9GSZKC9CZ7i5D8jBzJe7ZZtsySfy7BcB7yko3bhHWbpcVX9Unb',
        },
        {
            path: "m/44'/501'/2'/0'",
            rawTx: 'QwE1XMGNhYCYcry9Y8tupaw8KBShus8uUNjsYjZyEZF7eezR8amPoNeT18uS69mkedxqugroVwJTqofA2jLbXtK9QB87vuUsK8VXYf2Ehvri4o9GSZKC9CZ7i5D8jBzJe7ZZtsySfy7BcB7yko3bhHWbpcVX9Unb',
        },
    ]
});
```

#### Result

```javascript
{
    success: true,
    payload: {
        signature: string
    }
}
```

Result with bundle

```javascript
{
    success: true,
    payload: [
        { signature: string }, // account 1
        { signature: string }, // account 2
        { signature: string }, // account 3
    ]
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

#### Error Detail

1. firmware not support, must upgrade to the latest version.

```json
{
  "success": false,
  "payload": {
    "error": "Unknown message",
    "code": "Failure_UnexpectedMessage"
  }
}
```

\<Playground initValue={ `OneKeyConnect.solanaSignTransaction({ path: "m/44'/501'/0'/0'", rawTx: 'QwE1XMGNhYCYcry9Y8tupaw8KBShus8uUNjsYjZyEZF7eezR8amPoNeT18uS69mkedxqugroVwJTqofA2jLbXtK9QB87vuUsK8VXYf2Ehvri4o9GSZKC9CZ7i5D8jBzJe7ZZtsySfy7BcB7yko3bhHWbpcVX9Unb', });`} />
