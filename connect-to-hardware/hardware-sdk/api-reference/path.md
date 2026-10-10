# Path Parameters

A derivation path identifies a key within a wallet. The SDK accepts a string such as `m/44'/0'/0'/0/0` or an array of unsigned 32-bit integers.

An apostrophe marks a hardened component. In an integer array, add `0x80000000` to that component. Non-hardened components keep their original index.

## Bitcoin examples

BIP44 account 0:

```typescript
const accountPath = "m/44'/0'/0'";
const accountPathArray = [0x8000002c, 0x80000000, 0x80000000];
```

The first receiving address in that account:

```typescript
const addressPath = "m/44'/0'/0'/0/0";
const addressPathArray = [0x8000002c, 0x80000000, 0x80000000, 0, 0];
```

For Bitcoin, use purpose `49'` for nested SegWit, `84'` for native SegWit, or `86'` for Taproot. Account and address indexes start at zero.

## Chain-specific paths

Use the path format documented for the chain and the wallet account being imported. For example, Solana commonly uses the fully hardened path `m/44'/501'/0'/0'`.

Do not harden or unharden a component merely to satisfy a length check: doing so changes the derived key. Cardano uses BIP32-Ed25519 and has its own payment and staking path rules; it is not covered by a blanket rule that every Ed25519 path must be fully hardened.

See the [BIP44 specification](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) and the individual API's path requirements.
