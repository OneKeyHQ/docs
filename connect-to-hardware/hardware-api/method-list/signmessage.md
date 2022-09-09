# signMessage

import Playground from "@src/components/playground";

### Sign message

Asks device to sign a message using the private key derived by given BIP32 path.

ES6

```javascript
const result = await OneKeyConnect.signMessage(params);
```

CommonJS

```javascript
OneKeyConnect.signMessage(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L131-L135)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `message` - _required_ `string`
* `coin` - _optional_ `string` Determines network definition specified in [coins.json](https://github.com/OneKeyHQ/connect/blob/onekey/src/data/coins.json) file. Coin `shortcut`, `name` or `label` can be used. If `coin` is not set API will try to get network definition from `path`.
* `hex` - _optional_ `boolean` convert message from hex

#### Example

```javascript
OneKeyConnect.signMessage({
    path: "m/44'/0'/0'",
    message: "example message"
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L113-L116)

```javascript
{
    success: true,
    payload: {
        address: string,   // signer address
        signature: string, // signature in base64 format
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

\<Playground initValue={ `OneKeyConnect.signMessage({ path: "m/44'/0'/0'", message: "example message" });`} />
