# ethereumGetAddress

import Playground from "@src/components/playground";

### Ethereum712: sign message

Asks device to sign a message using the private key derived by given BIP32 path.

ES6

```javascript
const result = await OneKeyConnect.ethereumSignMessage(params);
```

CommonJS

```javascript
OneKeyConnect.ethereumSignMessage(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L64-L67)

* `path` - _required_ `string | Array<number>` minimum length is `3`. read more
* `version` - _optional_ `string` `V3` or `V4`, for `sign_typed_data` version, default is V3.
* `data` - _required_ `object`, message data type. see example for more detail.

#### Example

```javascript
OneKeyConnect.ethereumSignMessage712({
    path: "m/44'/60'/0'",
    data: {
        domain: {
            name: 'My amazing dApp',
            version: '2',
            chainId: '0x01',
            verifyingContract: 'xx',
            salt: 'xx',
        },
        message: {
            amount: 100,
            bidder: {
                userId: 323,
                wallet: '0x3333333333333333333333333333333333333333',
            },
        },
        primaryType: 'Bid',
        types: {
            EIP712Domain: [
                { name: 'name', type: 'string' },
                { name: 'version', type: 'string' },
                { name: 'chainId', type: 'uint256' },
                { name: 'verifyingContract', type: 'address' },
                { name: 'salt', type: 'bytes32' },
            ],
            Bid: [
                { name: 'amount', type: 'uint256' },
                { name: 'bidder', type: 'Identity' },
            ],
            Identity: [
                { name: 'userId', type: 'uint256' },
                { name: 'wallet', type: 'address' },
            ],
        },
    }
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/response.js#L47-L50)

```javascript
{
    success: true,
    payload: {
        address: string,
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

1. pass an illegal parameter

```json
{
  "success": false,
  "payload": {
    "error": "Parameter \"domain\" is missing.",
    "code": "Method_InvalidParameter"
  }
}

{
  "success": false,
  "payload": {
    "error": "Parameter \"message\" is missing.",
    "code": "Method_InvalidParameter"
  }
}
```

1. parameter must be 32 bytes, if it too short or too long

```json
{
    {
        "success": false,
        "payload": {
            "error": "data length error",
            "code": "Failure_ProcessError"
        }
    }
}

{
  "success": false,
  "payload": {
    "error": "Illegal str: Length not a multiple of 2"
  }
}

{
  "success": false,
  "payload": {
    "error": "bytes overflow",
    "code": "Failure_DataError"
  }
}
```

1. not enable EIP712 feature

```json
{
  "success": false,
  "payload": {
    "error": "EIP712 blind sign is disabled",
    "code": "Failure_ProcessError"
  }
}
```

1. not support EIP712Domain primary type, your passed data's `primaryType` must not be `EIP712Domain`

```json
{
  "success": false,
  "payload": {
    "error": "primaryType `EIP712Domain` is not support",
    "code": "Backend_NotSupported"
  }
}
```

\<Playground initValue={ `OneKeyConnect.ethereumSignMessageEIP712({ path: "m/44'/60'/0'", data: { domain: { name: 'My amazing dApp', version: '2', chainId: '0x01', verifyingContract: '0x1C56346CD2A2Bf3202F771f50d3D14a367B48070', salt: '0xf2d857f4a3edcb9b78b4d503bfe733db1e3f6cdc2b7971ee739626c97e86a558', }, message: { amount: 100, bidder: { userId: 323, wallet: '0x3333333333333333333333333333333333333333', }, }, primaryType: 'Bid', types: { EIP712Domain: [ { name: 'name', type: 'string' }, { name: 'version', type: 'string' }, { name: 'chainId', type: 'uint256' }, { name: 'verifyingContract', type: 'address' }, { name: 'salt', type: 'bytes32' }, ], Bid: [ { name: 'amount', type: 'uint256' }, { name: 'bidder', type: 'Identity' }, ], Identity: [ { name: 'userId', type: 'uint256' }, { name: 'wallet', type: 'address' }, ], }, } });`} />
