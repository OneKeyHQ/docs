# rippleSignTransaction

import Playground from "@src/components/playground";

### Ripple: Sign transaction

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

ES6

```javascript
const result = await OneKeyConnect.rippleSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.rippleSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L149-L154)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `Object` type of [RippleTransaction](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/ripple.js#L36-L42)

#### Example

```javascript
OneKeyConnect.rippleSignTransaction(
    path: "m/44'/144'/0'/0/0",
    transaction: {
        fee: '100000',
        flags: 0x80000000,
        sequence: 25,
        payment: {
            amount: '100000000',
            destination: 'rBKz5MC2iXdoS3XgnNSYmF69K1Yo4NS3Ws'
        }
    }
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/ripple.js#L49-L52)

```javascript
{
    success: true,
    payload: {
        serializedTx: string,
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

\<Playground initValue={ `OneKeyConnect.rippleSignTransaction( path: "m/44'/144'/0'/0/0", transaction: { fee: '100000', flags: 0x80000000, sequence: 25, payment: { amount: '100000000', destination: 'rBKz5MC2iXdoS3XgnNSYmF69K1Yo4NS3Ws' } } });`} />
