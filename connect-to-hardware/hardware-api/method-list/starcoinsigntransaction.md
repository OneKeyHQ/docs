# starcoinSignTransaction

import Playground from "@src/components/playground";

### Starcoin: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

:::warning warning

* This method is supported after OneKey Connect JS-SDK version v8.6.0, and npm modules equal or larger than this version can be used.
* The OneKey device firmware may also be different for the user, caller should handle the response of method like firmware not support issue. :::

ES6

```javascript
const result = await OneKeyConnect.starcoinSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.starcoinSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `rawTx` - _required_ string.

#### Example

```javascript
OneKeyConnect.starcoinSignTransaction(
    path: "m/44'/101010'/0'/0'/0'",
    rawTx: '',
);
```

#### Result

```javascript
{
    success: true,
    payload: {
        signature: string,
        public_key: string,
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

\<Playground initValue={ `OneKeyConnect.starcoinSignTransaction({ path: "m/44'/101010'/0'/0'/0'", rawTx: '', });`} />
