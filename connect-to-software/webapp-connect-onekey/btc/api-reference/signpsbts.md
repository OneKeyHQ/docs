# signPsbts

## signPsbts

Sign multiple PSBTs at once. This method will traverse all inputs that match the current address to sign.

#### Wallet & Address Type Compatibility

| Wallet Type     | Model          | Address Types Supported                |
| --------------- | -------------- | -------------------------------------- |
| Software Wallet | All            | p2wpkh (Native Segwit), p2tr (Taproot) |
| Hardware Wallet | Pro, Classic1S | p2tr (Taproot)                         |

#### Method

```typescript
async function signPsbts(
    psbtHexs: string[],
    options?: {
        autoFinalized?: boolean;
        toSignInputs?: Array<{
            index: number;
            address?: string;
            publicKey?: string;
            sighashTypes?: number[];
            useTweakedSigner?: boolean;
        }>;
    },
): Promise<string[]>
```

#### Parameters

* `psbtHexs` — _required_ `string[]` Array of hex strings of PSBTs to sign
* `options` — _optional_ `object`
  * `autoFinalized` — _optional_ `boolean` Whether to finalize PSBTs after signing, default is `true`
  * `toSignInputs` — _optional_ `array` Specify which inputs to sign
    * `index` — _required_ `number` Which input to sign
    * `address` — _optional_ `string` Which corresponding private key to use for signing (specify either address or publicKey)
    * `publicKey` — _optional_ `string` Which corresponding private key to use for signing (specify either address or publicKey)
    * `sighashTypes` — _optional_ `number[]` Optional sighash types
    * `useTweakedSigner` — _optional_ `boolean` Force whether to use tweaked signer, has higher priority than disableTweakSigner

#### Returns

* `Promise<string[]>` Array of hex strings of signed PSBTs

#### Example

```typescript
const provider = (window.$onekey && window.$onekey.btc) || window.unisat;

try {
    const psbtHexs = [
        "70736274ff01007d...",
        "70736274ff01007d..."
    ];

    const signeds = await provider.signPsbts(psbtHexs, {
        autoFinalized: true,
        toSignInputs: [{
            index: 0,
            publicKey: "02abc...",
            useTweakedSigner: true
        }]
    });

    console.log(signeds);
} catch (e) {
    console.log(e);
}
```

#### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/XWOPaPY" %}
