# liskSignMessage

import Playground from "@src/components/playground";

### Lisk: Sign message

Asks device to sign a message using the private key derived by given BIP32 path.

ES6

```javascript
const result = await OneKeyConnect.liskSignMessage(params);
```

CommonJS

```javascript
OneKeyConnect.liskSignMessage(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#L109-L112)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `message` - _required_ `string` message to sign in plain text

#### Example

```javascript
OneKeyConnect.liskSignMessage({
    path: "m/44'/134'/0'",
    message: "example message"
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L104-L106)

```javascript
{
    success: true,
    payload: {
        publicKey: string,
        signature: string,
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

\<Playground initValue={ `OneKeyConnect.liskSignMessage({ path: "m/44'/134'/0'", message: "example message" });`} />
