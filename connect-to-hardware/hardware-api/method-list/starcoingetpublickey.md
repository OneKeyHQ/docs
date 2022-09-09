# starcoinGetPublicKey

import Playground from "@src/components/playground";

### Starcoin: Get public key

Retrieves BIP32 extended public derived by given BIP32 path. User is presented with a description of the requested key and asked to confirm the export on OneKey.

:::warning warning

* This method is supported after OneKey Connect JS-SDK version v8.6.0, and npm modules equal or larger than this version can be used.
* The OneKey device firmware may also be different for the user, caller should handle the response of method like firmware not support issue. :::

ES6

```javascript
const result = await OneKeyConnect.starcoinGetPublicKey(params);
```

CommonJS

```javascript
OneKeyConnect.starcoinGetPublicKey(params).then(function(result) {

});
```

#### Params

**Optional common params**

**Exporting single address**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `showOnDevice` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `false`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnDevice` fields

#### Example

Display address of first starcoin account:

```javascript
OneKeyConnect.starcoinGetPublicKey({
    path: "m/44'/101010'/0'/0'/0'"
});
```

Return a bundle of starcoin addresses without displaying them on device:

```javascript
OneKeyConnect.starcoinGetPublicKey({
    bundle: [
        { path: "m/44'/101010'/0'/0'/0'", showOnDevice: false }, // account 1
        { path: "m/44'/101010'/0'/0'/1'", showOnDevice: false }, // account 2
        { path: "m/44'/101010'/0'/0'/2'", showOnDevice: false }  // account 3
    ]
});
```

#### Result

Result with only one address

```javascript
{
    success: true,
    payload: {
        publicKey: string,     // displayed address
        path: Array<number>, // hardended path
        serializedPath: string,
    }
}
```

Result with bundle of addresses sorted by FIFO

```javascript
{
    success: true,
    payload: [
        { publicKey: string, path: Array<number>, serializedPath: string }, // account 1
        { publicKey: string, path: Array<number>, serializedPath: string }, // account 2
        { publicKey: string, path: Array<number>, serializedPath: string }  // account 3
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

\<Playground initValue={ `OneKeyConnect.starcoinGetPublicKey({ path: "m/44'/101010'/0'/0'/0'" });`} />
