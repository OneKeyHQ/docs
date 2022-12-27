# cardanoGetAddress

## Cardano: get address

Display requested address derived by given BIP32-Ed25519 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.cardanoGetAddress(connectId, deviceId, params)
```

### Params

[Optional common params](../common-params.md)

**Exporting single address**

* `addressParameters` — _required_ see description below
* `address` — _optional_ `string` address for validation (read `Handle button request` section below)
* `protocolMagic` - _required_ `Integer` 764824073 for Mainnet, 1 for Preprod Testnet, 2 for Preview Testnet
* `networkId` - _required_ `Integer` 1 for Mainnet, 0 for Testnet
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `derivationType` — _optional_ `CardanoDerivationType` enum. determines used derivation type. Default is set to ICARUS=1
* `isCheck` — _optional_ `boolean` checks only the base address

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with single address fields

**Address Parameters**

* `addressType` - _required_ `CardanoAddressType`/`number` - you can use the flow `CARDANO.ADDRESS_TYPE` object or typescript `CardanoAddressType` enum. Supports all address types.
* `path` — _required_ `string | Array<number>` minimum length is `5`.
* `stakingPath` — _optional_ `string | Array<number>` minimum length is `5`. Used for base and reward address derivation
* `stakingKeyHash` - _optional_ `string` hex string of staking key hash. Used for base address derivation (as an alternative to `stakingPath`)
* `certificatePointer` - _optional_ CardanoCertificatePointer object. Must contain `number`s `blockIndex`, `txIndex` and `certificateIndex`. Used for pointer address derivation.
* `paymentScriptHash` - _optional_ `string` hex string of payment script hash.
* `stakingScriptHash` - _optional_ `string` hex string of staking script hash.



### Example

Display byron address of first cardano account:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.BYRON,
        path: "m/44'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display base address of first cardano account:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.BASE,
        path: "m/1852'/1815'/0'/0/0",
        stakingPath: "m/1852'/1815'/0'/2/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display base address with script payment part:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.BASE_SCRIPT_KEY,
        paymentScriptHash: '0d5acbf6a1dfb0c8724e60df314987315ccbf78bb6c0f9b6f3d568fe',
        stakingPath: "m/1852'/1815'/0'/2/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display base address with script staking part:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.BASE_KEY_SCRIPT,
        path: "m/1852'/1815'/0'/0/0",
        stakingScriptHash: '8d7bebc7a58f1c7b5fb7c9391071ecd3b51b032695522f8c555343a9',
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display base address with both payment and staking part being a script:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.BASE_SCRIPT_SCRIPT,
        paymentScriptHash: '0d5acbf6a1dfb0c8724e60df314987315ccbf78bb6c0f9b6f3d568fe',
        stakingScriptHash: '8d7bebc7a58f1c7b5fb7c9391071ecd3b51b032695522f8c555343a9',
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display pointer address of first cardano account:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.POINTER,
        path: "m/1852'/1815'/0'/0/0",
        certificatePointer: {
            blockIndex: 1,
            txIndex: 2,
            certificateIndex: 3,
        },
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display pointer script address:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.POINTER_SCRIPT,
        paymentScriptHash: '0d5acbf6a1dfb0c8724e60df314987315ccbf78bb6c0f9b6f3d568fe',
        certificatePointer: {
            blockIndex: 1,
            txIndex: 2,
            certificateIndex: 3,
        },
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display enterprise address of first cardano account:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.ENTERPRISE,
        path: "m/1852'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display enterprise script address:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.ENTERPRISE_SCRIPT,
        paymentScriptHash: '0d5acbf6a1dfb0c8724e60df314987315ccbf78bb6c0f9b6f3d568fe',
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display reward address of first cardano account:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.REWARD,
        stakingPath: "m/1852'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display reward script address:

```javascript
HardwareSDK.cardanoGetAddress({
    addressParameters: {
        addressType: CardanoAddressType.REWARD_SCRIPT,
        stakingScriptHash: '8d7bebc7a58f1c7b5fb7c9391071ecd3b51b032695522f8c555343a9',
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Return a bundle of cardano addresses without displaying them on device:

```javascript
HardwareSDK.cardanoGetAddress({
    bundle: [
        // byron address, account 1, address 1
        {
            addressParameters: {
                addressType: 8,
                path: "m/44'/1815'/0'/0/0",
            },
            protocolMagic: 764824073,
            networkId: 1,
            showOnOneKey: false,
        },
        // base address with staking key hash, account 1, address 1
        {
            addressParameters: {
                addressType: 0,
                path: "m/1852'/1815'/0'/0/0",
                stakingKeyHash: '1bc428e4720702ebd5dab4fb175324c192dc9bb76cc5da956e3c8dff',
            },
            protocolMagic: 764824073,
            networkId: 1,
            showOnOneKey: false,
        },
        // byron address, account 2, address 3, testnet
        {
            addressParameters: {
                addressType: 8,
                path: "m/44'/1815'/1'/0/2",
            },
            protocolMagic: 1,
            networkId: 0,
            showOnOneKey: false,
        },
    ],
});
```

#### Result <a href="#result" id="result"></a>

Result with only one address

```javascript
{
    success: true,
    payload: {
        addressParameters: {
            addressType: number,
            path: Array<number>, // hardend path
            stakingPath?: Array<number>, // hardend path
            stakingKeyHash?: string,
            certificatePointer?: {
                blockIndex: number,
                txIndex: number,
                certificatePointer: number,
            },
            paymentScriptHash?: string,
            stakingScriptHash?: string,
        }
        serializedPath?: string,
        serializedStakingPath?: string,
        protocolMagic: number,
        networkId: number,
        address: string,
        xpub: string,
        stakeAddress: string, // If a base address is created, the stake address is created together
    }
}
```

Result with bundle of addresses

```javascript
{
    success: true,
    payload: [
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                },
                paymentScriptHash?: string,
                stakingScriptHash?: string,
            }
            serializedPath?: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                },
                paymentScriptHash?: string,
                stakingScriptHash?: string,
            }
            serializedPath?: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                },
                paymentScriptHash?: string,
                stakingScriptHash?: string,
            }
            serializedPath?: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
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
