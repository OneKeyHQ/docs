# tezosGetPublicKey

import Playground from "@src/components/playground";

### Tezos: Get public key

Display requested public key on device and returns it to caller. User is presented with a description of the requested public key and asked to confirm the export.

ES6

```javascript
const result = await OneKeyConnect.tezosGetPublicKey(params);
```

CommonJS

```javascript
OneKeyConnect.tezosGetPublicKey(params).then(function(result) {

});
```

#### Params

**Optional common params**

**Exporting single public key**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `showOnOneKey` — _optional_ `boolean` determines if public key will be displayed on device.

**Exporting bundle of public keys**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Result with only one public key

```javascript
OneKeyConnect.tezosGetPublicKey({
    path: "m/49'/1729'/0'",
});
```

Result with bundle of public keys

```javascript
OneKeyConnect.tezosGetPublicKey({
    bundle: [
        { path: "m/49'/1729'/0'" }, // account 1
        { path: "m/49'/1729'/1'" }, // account 2
        { path: "m/49'/1729'/2'" }  // account 3
    ]
});
```

#### Result

Result with only one public key

```javascript
{
    success: true,
    payload: {
        publicKey: string,
        path: Array<number>,
        serializedPath: string,
    }
}
```

Result with bundle of public keys

```javascript
{
    success: true,
    payload: [
        { path, serializedPath, publicKey }, // account 1
        { path, serializedPath, publicKey }, // account 2
        { path, serializedPath, publicKey }, // account 3
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

\<Playground initValue={ `OneKeyConnect.tezosGetPublicKey({ path: "m/49'/1729'/0'", });`} />
