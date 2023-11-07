# Path Params

## Path

* `path` - `string | Array<number>` in [BIP44](https://github.com/OneKeyHQ/bixin-firmware/blob/bixin\_dev/docs/misc/coins-bip44-paths.md) path scheme or `Array` of hardended numbers.

> If the chain is ed25519, it must be fully hardened.
>
> e.g m/49'/0/'0'/'0'/'0'

#### Examples

Bitcoin account 1 using BIP44 derivation path

```javascript
"m/49'/0/'0'"
```

Bitcoin account 1 using hardended path

```javascript
[(49 | 0x80000000) >>> 0, (0 | 0x80000000 >>> 0), (0 | 0x80000000) >>> 0]
```

Bitcoin first address address of account 1 using BIP44 derivation path

```javascript
"m/49'/0/'0'/0/0"
```

Bitcoin first address address of account 1 using hardended path

```javascript
[(49 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, 0, 0]
```

[See more examples](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki#examples)
