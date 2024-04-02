# CryptoKeypath

The `CryptoKeypath` class represents a cryptographic key path.

### Parameters

* `components`: `PathComponent[]` An array of path components, default is an empty array.
* `sourceFingerprint`: `Buffer` The source fingerprint, optional.
* `depth`: `number` The depth, optional.



### `PathComponent`

The `PathComponent` class represents an individual component in a cryptographic key path.

#### Parameters

* `args`: Object
  * `index`: `number` The component index, optional.
  * `hardened`: `boolean` Indicates whether it is hardened.



## URL Example

```typescript
ur:crypto-keypath/xxxxx
```

