# evmSignTypedData

## Ethereum: Sign Typed Data

Asks device to sign an [EIP-712](https://eips.ethereum.org/EIPS/eip-712) typed data message using the private key derived by given BIP32 path.

User is asked to confirm all signing details on OneKey device.

```typescript
const result = await HardwareSDK.evmSignTypedData(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `data` - _required_ `Object` type of `EthereumSignTypedDataMessage`\`. A JSON Schema definition can be found in the EIP-712 spec.
* `metamaskV4Compat` - _required_ `boolean` set to `true` for compatibility with [MetaMask's signTypedData\_v4](https://docs.metamask.io/guide/signing-data.html#sign-typed-data-v4).
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain.

### Blind signing

You may also wish to contruct your own hashes using a different library.

* `domainHash` - _required_ `string` hex-encoded 32-byte hash of the EIP-712 domain.
* `messageHash` - _optional_ `string` hex-encoded 32-byte hash of the EIP-712 message. This is optional for the domain-only hashes where `primaryType` is `EIP712Domain`.

### Example

```typescript
const eip712Data = {
    types: {
        EIP712Domain: [
            {
                name: 'name',
                type: 'string',
            },
        ],
        Message: [
            {
                name: "Best Wallet",
                type: "string"
            },
            {
                name: "Number",
                type: "uint64"
            }
        ]
    },
    primaryType: 'Message',
    domain: {
        name: 'example.onekey.io',
    },
    message: {
        "Best Wallet": "OneKey Wallet",
        // be careful with JavaScript numbers: MAX_SAFE_INTEGER is quite low
        "Number": `${2n ** 55n}`,
    },
};

const {domainHash, messageHash} = transformTypedDataPlugin(eip712Data, true);

HardwareSDK.evmSignTypedData(connectId, deviceId, {
    path: "m/44'/60'/0'",
    data: eip712Data,
    metamaskV4Compat: true,
    domainHash,
    messageHash,
    chainId: 1
});
```

#### Result

```typescript
{
    success: true,
    payload: {
        address: string,
        signature: string, // hexadecimal string with "0x" prefix
    }
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
