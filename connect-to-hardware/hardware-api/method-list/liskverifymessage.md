# liskVerifyMessage

import Playground from '@src/components/playground';

### Lisk: Verify message

Asks device to verify a message using the signer public key and signature.

ES6

```javascript
const result = await OneKeyConnect.liskVerifyMessage(params);
```

CommonJS

```javascript
OneKeyConnect.liskVerifyMessage(params).then(function (result) {});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#L133-L137)

* `publicKey` - _required_ `string` signer public key.
* `signature` - _required_ `string` message signature
* `message` - _required_ `string` signed message in plain text

#### Example

```javascript
OneKeyConnect.ethereumVerifyMessage({
  publicKey: 'bf70027c9e4cea4584bd6016748c21e350708b2c166bf61ea78a147b5ff320ae',
  signature:
    'd39843f39983cf42609d1667f1c5a7958d8aef6b06880b93f67833630113a11c6847607a184d17da24bfaf799afc45fdcf2abef34142a23cabeb0d11374ac103',
  message: 'example message',
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/lisk.js#L139-L142)

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

\<Playground initValue={`OneKeyConnect.ethereumVerifyMessage({ publicKey: "bf70027c9e4cea4584bd6016748c21e350708b2c166bf61ea78a147b5ff320ae", signature: "d39843f39983cf42609d1667f1c5a7958d8aef6b06880b93f67833630113a11c6847607a184d17da24bfaf799afc45fdcf2abef34142a23cabeb0d11374ac103", message: "example message", });`} />
