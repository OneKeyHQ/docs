# cardanoGetPublicKey

import Playground from "@src/components/playground";

### Cardano: Get public key

Retrieves [BIP32-Ed25519](https://cardanolaunch.com/assets/Ed25519\_BIP.pdf) extended public derived by given [BIP32-Ed25519](https://cardanolaunch.com/assets/Ed25519\_BIP.pdf) path. User is presented with a description of the requested key and asked to confirm the export on OneKey.

ES6

```javascript
const result = await OneKeyConnect.cardanoGetPublicKey(params);
```

CommonJS

```javascript
OneKeyConnect.cardanoGetPublicKey(params).then(function(result) {

});
```

#### Params

**Optional common params**

**Exporting single public key**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `showOnOneKey` — _optional_ `boolean` determines if publick key will be displayed on device. Default is set to `true`

**Exporting bundle of publick keys**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display public key of first cardano account:

```javascript
OneKeyConnect.cardanoGetPublicKey({
    path: "m/44'/1815'/0'"
});
```

Return a bundle of cardano public keys without displaying them on device:

```javascript
OneKeyConnect.cardanoGetPublicKey({
    bundle: [
        { path: "m/44'/1815'/0'", showOnOneKey: false }, // account 1
        { path: "m/44'/1815'/1'", showOnOneKey: false }, // account 2
        { path: "m/44'/1815'/2'", showOnOneKey: false }  // account 3
    ]
});
```

#### Result

Result with only one public key

```javascript
{
    success: true,
    payload: {
        path: Array<number>, // hardended path
        serializedPath: string,
        publicKey: string,
        node: HDPubNode,
        rootHDPassphrase: string,
    }
}
```

Result with bundle of publick keys

```javascript
{
    success: true,
    payload: [
        { path: Array<number>, serializedPath: string, publicKey: string, node: HDPubNode, hdPassphrase: string }, // account 1
        { path: Array<number>, serializedPath: string, publicKey: string, node: HDPubNode, rootHDPassphrase: string }, // account 2
        { path: Array<number>, serializedPath: string, publicKey: string, node: HDPubNode, hdPassphrase: string }  // account 3
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

\<Playground initValue={ `OneKeyConnect.cardanoGetPublicKey({ path: "m/44'/1815'/0'" });`} />
