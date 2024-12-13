# signPsbt

Sign PSBT

This method will traverse all inputs that match the current address to sign.

### Wallet & Address Type Compatibility

| Wallet Type     | Model       | Address Types Supported                  |
|-----------------|-------------|------------------------------------------|
| Software Wallet | All         | p2wpkh (Native Segwit), p2tr (Taproot)   |
| Hardware Wallet | Pro, Classic1S | p2tr (Taproot)                        |

### Method

```typescript
async function signPsbt(
    psbtHex: string,
    options?: {
        autoFinalized?: boolean;
        toSignInputs?: Array<{
            index: number;
            address?: string;
            publicKey?: string;
            sighashTypes?: number[];
            disableTweakSigner?: boolean;
            useTweakedSigner?: boolean;
        }>;
    }
): Promise<string>
```

### Params

* `psbtHex` — _required_ `string` the hex string of psbt to sign
* `options` — _optional_ `object`
  * `autoFinalized` — _optional_ `boolean`: whether finalize psbt after signing, default is `true`
  * `toSignInputs` — _optional_ `Array`: specify which inputs to sign
    * `index` — _required_ `number`: which input to sign
    * `address` — _optional_ `string`: (specify either address or publicKey) which corresponding private key to use for signing
    * `publicKey` — _optional_ `string`: (specify either address or publicKey) which corresponding private key to use for signing
    * `sighashTypes` — _optional_ `number[]`: optional sighash types for the input
    * `disableTweakSigner` — _optional_ `boolean`: default is false. Set true to use original private key when signing taproot inputs
    * `useTweakedSigner` — _optional_ `boolean`: force whether to use tweaked signer. Higher priority than disableTweakSigner

### Returns

`Promise<string>` — the hex string of signed psbt

### Example

```typescript
const provider = (window.$onekey && window.$onekey.btc) || window.unisat;

try {
    // Basic signing
    const signedBasic = await provider.signPsbt("70736274ff01007d....");

    // Advanced signing with options
    const signedAdvanced = await provider.signPsbt(
        "70736274ff01007d....",
        {
            autoFinalized: false,
            toSignInputs: [
                {
                    index: 0,
                    address: "bc1qxxx...xxx",
                },
                {
                    index: 1,
                    publicKey: "02xxx...xxx",
                    sighashTypes: [1]
                },
                {
                    index: 2,
                    publicKey: "02xxx...xxx",
                    disableTweakSigner: true
                }
            ]
        }
    );
    console.log('Signed PSBT:', signedAdvanced);
} catch (error) {
    console.error('Failed to sign PSBT:', error);
    // Handle signing errors appropriately
}
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/GRzXvXJ" %}
