# eosSignTransaction

import Playground from "@src/components/playground";

### EOS: Sign transaction

Asks device to sign given transaction using the private key derived by given BIP44 path. User is asked to confirm all transaction details on OneKey.

ES6

```javascript
const result = await OneKeyConnect.eosSignTransaction(params);
```

CommonJS

```javascript
OneKeyConnect.eosSignTransaction(params).then(function(result) {

});
```

#### Params

**Optional common params**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/params.js#L69-L72)

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `transaction` - _required_ `Object` type of [Transaction](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/eos.js#L145-L149)

#### Transfer example

```javascript
OneKeyConnect.eosSignTransaction({
    path: "m/44'/194'/0'/0/0",
    transaction: {
        chainId: 'cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f',
        header: {
            expiration: '2019-12-01T12:00:00',
            refBlockNum: 6439,
            refBlockPrefix: 2995713264,
            maxNetUsageWords: 0,
            maxCpuUsageMs: 0,
            delaySec: 0,
        },
        actions: [{
            account: 'eosio.token',
            authorization: [{
                actor: 'abcdefg12345',
                permission: 'active',
            }],
            name: 'transfer',
            data: {
                from: 'abcdefg12345',
                to: '12345abcdefg',
                quantity: '1.0000 EOS',
                memo: 'memo',
            },
        }]
    }
});
```

#### Result

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/eos.js#L160-L163)

```javascript
{
    success: true,
    payload: {
        signature: string, // hexadecimal string
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

\<Playground initValue={ `OneKeyConnect.eosSignTransaction({ path: "m/44'/194'/0'/0/0", transaction: { chainId: 'cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f', header: { expiration: '2019-12-01T12:00:00', refBlockNum: 6439, refBlockPrefix: 2995713264, maxNetUsageWords: 0, maxCpuUsageMs: 0, delaySec: 0, }, actions: [{ account: 'eosio.token', authorization: [{ actor: 'abcdefg12345', permission: 'active', }], name: 'transfer', data: { from: 'abcdefg12345', to: '12345abcdefg', quantity: '1.0000 EOS', memo: 'memo', }, }] } });`} />
