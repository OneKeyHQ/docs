# tonSignMessage

### Use requirement

* Firmware version required
  * Touch: 4.10.0

### TON: sign message

Sign a message or transaction using the private key derived by given BIP32 path. User needs to confirm the action on OneKey device.

```typescript
const response = await HardwareSDK.tonSignMessage(connectId, deviceId, {
    path: "m/44'/607'/0'",
    destination: "UQBYkuShkZzRYAWX_HrK3kFpeAixiRKd-K7QBXYxl9OBXM0_",
    tonAmount: 100,
    seqno: 0,
    expireAt: Date.now() + 1000 * 60 * 60 * 24,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false
});
```

#### Params

[Optional common params](../../../hardware-sdk/api-reference/common-params.md)

**Required Parameters**

* `path` — _required_ `string | Array<number>` minimum length is 3
* `destination` — _required_ `string` destination address
* `tonAmount` — _required_ `number` amount of TON to transfer
* `seqno` — _required_ `number` sequence number
* `expireAt` — _required_ `number` message expiration timestamp

**Optional Parameters**

* `walletVersion` — _optional_ `TonWalletVersion` TON wallet version (default: 3)
* `isBounceable` — _optional_ `boolean` whether message is bounceable (default: false)
* `isTestnetOnly` — _optional_ `boolean` whether to use testnet (default: false)
* `jettonMasterAddress` — _optional_ `string` Jetton master contract address
* `jettonAmount` — _optional_ `number` amount of jettons to transfer
* `fwdFee` — _optional_ `number` forward fee for jetton transfer
* `comment` — _optional_ `string` message comment
* `mode` — _optional_ `number` message mode
* `workchain` — _optional_ `TonWorkChain` TON workchain
* `walletId` — _optional_ `number` wallet ID

#### Example

<pre class="language-typescript"><code class="lang-typescript"><strong>// Simple TON transfer
</strong>const response = await HardwareSDK.tonSignMessage(connectId, deviceId, {
    path: "m/44'/607'/0'",
    destination: "UQBYkuShkZzRYAWX_HrK3kFpeAixiRKd-K7QBXYxl9OBXM0_",
    tonAmount: 100,
    seqno: 0,
    expireAt: Date.now() + 1000 * 60 * 60 * 24,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false
});

// Multiple Native
const jettonResponse = await HardwareSDK.tonSignMessage(connectId, deviceId, {
    path: "m/44'/607'/0'",
    destination: 'UQBYkuShkZzRYAWX_HrK3kFpeAixiRKd-K7QBXYxl9OBXM0_',
    tonAmount: 100,
    seqno: 0,
    expireAt: Date.now() + 1000 * 60 * 60 * 24,
    walletVersion: 3,
    isBounceable: false,
    isTestnetOnly: false,
    extDestination: ['UQBYkuShkZzRYAWX_HrK3kFpeAixiRKd-K7QBXYxl9OBXM0_'],
    extTonAmount: [100],
    extPayload: ['48656c6c6f204f6e654b6579'],
});
</code></pre>

Result

```typescript
{
    success: true,
    payload: {
        skip_validate: boolean; // Used to skip validation in classic 1s.
        signature: string;
        signning_message: string;
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
