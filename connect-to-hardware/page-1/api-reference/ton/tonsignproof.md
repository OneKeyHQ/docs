# tonSignProof

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### TON: sign proof

Sign a proof message for dApp authentication using the private key derived by given BIP32 path. User needs to confirm the action on OneKey device.

```typescript
const result = await HardwareSDK.tonSignProof(connectId, deviceId, {
    path: "m/44'/607'/0'",
    appdomain: "onekey.so",
    comment: "48656c6c6f204f6e654b6579",
    expireAt: Date.now() + 1000 * 60 * 60 * 24,
    walletVersion: "V4R2",
    isBounceable: false,
    isTestnetOnly: false
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is 3
* `appdomain` — _required_ `string` domain of the dApp requesting the proof
* `comment` — _optional_ `string` message comment in hex format
* `expireAt` — _required_ `number` proof expiration timestamp
* `walletVersion` — _optional_ `TonWalletVersion` TON wallet version (default: 3)
* `walletId` — _optional_ `number` wallet ID (default: 698983191)
* `workchain` — _optional_ `TonWorkChain` TON workchain (0: BaseChain, 1: MasterChain)
* `isBounceable` — _optional_ `boolean` whether message is bounceable (default: false)
* `isTestnetOnly` — _optional_ `boolean` whether to use testnet (default: false)

#### Returns

Response

* `signature` - proof signature in hex format

#### Example

```typescript
const response = await HardwareSDK.tonSignProof(connectId, deviceId, {
    path: "m/44'/607'/0'",
    appdomain: "onekey.so",
    comment: "48656c6c6f204f6e654b6579",
    expireAt: Date.now() + 1000 * 60 * 60 * 24,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
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
