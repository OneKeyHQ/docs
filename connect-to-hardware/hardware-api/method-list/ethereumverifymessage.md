# ethereumVerifyMessage

import Playground from "@src/components/playground";

### Ethereum: Verify message

Asks device to verify a message using the signer address and signature.

ES6

```javascript
const result = await OneKeyConnect.ethereumVerifyMessage(params);
```

CommonJS

```javascript
OneKeyConnect.ethereumVerifyMessage(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L74-L78)

* `address` - _required_ `string` signer address. "0x" prefix is optional
* `message` - _required_ `string` signed message in plain text
* `hex` - _optional_ `boolean` convert message from hex
* `signature` - _required_ `string` signature in hexadecimal format. "0x" prefix is optional

#### Example

```javascript
OneKeyConnect.ethereumVerifyMessage({
    address: "0xdA0b608bdb1a4A154325C854607c68950b4F1a34",
    message: "Example message",
    signature: "11dc86c631ef5d9388c5e245501d571b864af1a717cbbb3ca1f6dacbf330742957242aa52b36bbe7bb46dce6ff0ead0548cc5a5ce76d0aaed166fd40cb3fc6e51c",
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L133-L136)

```javascript
{
    success: true,
    payload: {
        message: "Message verified"
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

\<Playground initValue={ `OneKeyConnect.ethereumVerifyMessage({ address: "0xdA0b608bdb1a4A154325C854607c68950b4F1a34", message: "Example message", signature: "11dc86c631ef5d9388c5e245501d571b864af1a717cbbb3ca1f6dacbf330742957242aa52b36bbe7bb46dce6ff0ead0548cc5a5ce76d0aaed166fd40cb3fc6e51c", });`} />
