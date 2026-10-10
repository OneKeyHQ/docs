# Contract Method Names

Wallets may decode transaction call data to show a method name during confirmation. A function selector is the first four bytes of the Keccak-256 hash of its canonical signature, such as `transfer(address,uint256)`. See the [Solidity ABI specification](https://docs.soliditylang.org/en/latest/abi-spec.html#function-selector).

## Provide Verifiable Contract Metadata

Publish and verify your contract source and ABI on the relevant block explorer. Keep the ABI available to your application so it can encode calls correctly. A displayed method name alone is not proof of a contract's identity or behavior; selectors can collide.

## Legacy Signature Registries

Earlier versions of this guide described registering signatures in the Parity on-chain registry. The current provider source does not establish that OneKey uses that registry for transaction decoding. Do not pay for a registry transaction on the assumption that it will change the OneKey confirmation screen.

If a method name is missing, report the chain, contract address, method signature, transaction hash if available, and wallet version through the [OneKey issue tracker](https://github.com/OneKeyHQ/app-monorepo/issues). These details let the issue be reproduced without exposing private keys or recovery phrases.
