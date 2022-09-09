# nemSignTransaction

import Playground from "@src/components/playground";

### NEM: Sign transaction

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

ES6

```javascript
const result = await OneKeyConnect.nemSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.nemSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L114-L117)

* `path` - _required_ `string | Array<number>`
* `transaction` - _required_ `Object` type of [NEMTransaction](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/nem.js#L41)

#### Example

Sign simple transaction

```javascript
// common utility for hexlify message
function hexlify(str) {
    var result = '';
    var padding = '00';
    for (var i=0, l=str.length; i<l; i++) {
        var digit = str.charCodeAt(i).toString(16);
        var padded = (padding+digit).slice(-2);
        result += padded;
    }
    return result;
}

OneKeyConnect.nemSignTransaction(
    path: "m/44'/1'/0'/0'/0'",
    transaction: {
        timeStamp: 74649215,
        amount: 2000000,
        fee: 2000000,
        recipient: "TALICE2GMA34CXHD7XLJQ536NM5UNKQHTORNNT2J",
        type: 257,
        deadline: 74735615,
        version: (0x98 << 24),
        message: {
            payload: hexlify('test_nem_transaction_transfer'),
            type: 1,
        },
    }
});
```

Sign mosaic transaction

```javascript
OneKeyConnect.nemSignTransaction(
    path: "m/44'/1'/0'/0'/0'",
    transaction: {
        timeStamp: 76809215,
        amount: 1000000,
        fee: 1000000,
        recipient: "TALICE2GMA34CXHD7XLJQ536NM5UNKQHTORNNT2J",
        type: 257,
        deadline: 76895615,
        version: (0x98 << 24),
        message: {},
        mosaics: [
            {
                mosaicId: {
                    namespaceId: "nem",
                    name: "xem",
                },
                quantity: 1000000,
            }
        ]
    }
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L271-L274)

```javascript
{
    success: true,
    payload: {
        data: string,
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

\<Playground initValue={ `// common utility for hexlify message function hexlify(str) { var result = ''; var padding = '00'; for (var i=0, l=str.length; i<l; i++) { var digit = str.charCodeAt(i).toString(16); var padded = (padding+digit).slice(-2); result += padded; } return result; } OneKeyConnect.nemSignTransaction({ path: "m/44'/1'/0'/0'/0'", transaction: { timeStamp: 74649215, amount: 2000000, fee: 2000000, recipient: "TALICE2GMA34CXHD7XLJQ536NM5UNKQHTORNNT2J", type: 257, deadline: 74735615, version: (0x98 << 24), message: { payload: hexlify('test_nem_transaction_transfer'), type: 1, }, } });`} />
