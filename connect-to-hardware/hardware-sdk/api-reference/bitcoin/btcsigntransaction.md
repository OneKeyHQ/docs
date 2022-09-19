# btcSignTransaction

## Bitcoin: sign transaction

Asks device to sign given inputs and outputs of pre-composed transaction. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.btcSignTransaction(connectId, deviceId, params);
```

## Params

****[**Optional common params**](../common-params.md)****

* `coin` - _required_ `string` Determines network definition specified in coins.json file. Coin `shortcut`, `name` or `label` can be used.
* `inputs` - _required_ `Array` of TransactionInput,
* `outputs` - _required_ `Array` of TransactionOutput,
* `refTxs` - _optional_ `Array` of RefTransaction. If you don't want to use build-in `blockbook` backend you can optionally provide those data from your own backend transformed to `Onekey` format. Since Firmware 2.3.0/1.9.0 referenced transactions are required. Zcash and Komodo refTxs should also contains `expiry`, `version_group_id` and `extra_data` fields.
* `locktime` - _optional_ `number`,
* `version` - _optional_ `number` transaction version,
* `expiry` - _optional_ `number`, only for Decred and Zcash,
* `versionGroupId` - _optional_ `number` only for Zcash, nVersionGroupId when overwintered is set,
* `overwintered` - _optional_ `boolean` only for Zcash
* `timestamp` - _optional_ `number` only for Capricoin, transaction timestamp,
* `branchId` - _optional_ `number`, only for Zcash, BRANCH\_ID when overwintered is set

## Example

### PAYTOADDRESS

```typescript
HardwareSDK.signTransaction(connectId, deviceId, {
    inputs: [
        {
            address_n: [(44 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 0],
            prev_index: 0,
            prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac',
        }
    ],
    outputs: [
        {
            address_n: [(44 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 1],
            amount: '3181747',
            script_type: 'PAYTOADDRESS',
        }, {
            address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
            amount: '200000',
            script_type: 'PAYTOADDRESS',
        }
    ],
    coin: 'btc'
});
```

### SPENDP2SHWITNESS

```typescript
HardwareSDK.signTransaction({
    inputs: [
        {
            address_n: [(49 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 0],
            prev_index: 0,
            prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac',
            amount: '3382047',
            script_type: 'SPENDP2SHWITNESS',
        }
    ],
    outputs: [
        {
            address_n: [(49 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 1],
            amount: '3181747',
            script_type: 'PAYTOP2SHWITNESS',
        }, {
            address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
            amount: '200000',
            script_type: 'PAYTOADDRESS',
        }
    ],
    coin: 'btc'
});
```

### PAYTOADDRESS with refTxs (transaction data provided from custom backend)

```typescript
HardwareSDK.signTransaction({
    inputs: [
        {
            address_n: [(44 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 0],
            prev_index: 0,
            prev_hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac',
        }
    ],
    outputs: [
        {
            address_n: [(44 | 0x80000000) >>> 0, (0 | 0x80000000) >>> 0, (2 | 0x80000000) >>> 0, 1, 1],
            amount: '3181747',
            script_type: 'PAYTOADDRESS',
        }, {
            address: '18WL2iZKmpDYWk1oFavJapdLALxwSjcSk2',
            amount: '200000',
            script_type: 'PAYTOADDRESS',
        }
    ],
    refTxs: [
        {
            hash: 'b035d89d4543ce5713c553d69431698116a822c57c03ddacf3f04b763d1999ac',
            inputs: [
                {
                    prev_hash: '448946a44f1ef514601ccf9b22cc3e638c69ea3900b67b87517ea673eb0293dc',
                    prev_index: 0,
                    script_sig: '47304402202872cb8459eed053dcec0f353c7e293611fe77615862bfadb4d35a5d8807a4cf022015057aa0aaf72ab342b5f8939f86f193ad87b539931911a72e77148a1233e022012103f66bbe3c721f119bb4b8a1e6c1832b98f2cf625d9f59242008411dd92aab8d94',
                    sequence: 4294967295,
                }
            ],
            bin_outputs: [
                {
                    amount: 3431747,
                    script_pubkey: '76a91441352a84436847a7b660d5e76518f6ebb718dedc88ac',
                },
                {
                    amount: 10000,
                    script_pubkey: '76a9141403b451c79d34e6a7f6e36806683308085467ac88ac',
                }
            ],
            lock_time: 0,
            version: 1,
        },
    ],
    coin: 'btc'
});
```

### Result

```typescript
{
    success: true,
    payload: {
        signatures: Array<string>, // Array of signer signatures
        serializedTx: string,        // serialized transaction
        txid?: string,             // broadcasted transaction id
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
