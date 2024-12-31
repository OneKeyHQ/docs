# evmSignTransaction

### Ethereum: Sign transaction

* Use `evmSignTransaction` for signing transactions
* Supports both EIP-1559 and Legacy transactions. (But make sure that the RPC node you are using supports the type you are using)

Asks device to sign given transaction using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.evmSignTransaction(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `transaction` - _required_ `Object` type of `EthereumTransactionEIP1559|EthereumSignTransaction` "0x" prefix for each field is optional
* `chainId` - _optional_ `number` The ChainId in ETH is a unique identifier for a specific Ethereum network, used to distinguish different versions of the blockchain. [Reference](https://github.com/ethereum-lists/chains/tree/master/_data/chains).&#x20;

### Examples

#### EIP1559 ([after The London Upgrade](https://ethereum.org/en/developers/docs/gas/#post-london))

If both parameters `maxFeePerGas` and `maxPriorityFeePerGas` are defined, transaction will be signed as the new type introduced in [EIP1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md).

<pre class="language-typescript"><code class="lang-typescript"><strong>HardwareSDK.evmSignTransaction(connectId, deviceId, {
</strong>    path: "m/44'/60'/0'",
    transaction: {
        to: '0xd0d6d6c5fe4a677d343cc433536bb717bae167dd',
        value: '0xf4240',
        data: '0xa',
        chainId: 1,
        nonce: '0x0',
        maxFeePerGas: '0x14',
        maxPriorityFeePerGas: '0x0',
        gasLimit: '0x14',
    },
    chainId: 1
});
</code></pre>

Legacy

```typescript
HardwareSDK.evmSignTransaction(connectId, deviceId, {
    path: "m/44'/60'/0'",
    transaction: {
        to: "0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8",
        value: "0xf4240",
        data: "0x01",
        chainId: 1,
        nonce: "0x0",
        gasLimit: "0x5208",
        gasPrice: "0xbebc200"
    },
    chainId: 1
});
```

Result

```typescript
{
    success: true,
    payload: {
        v: string, // hexadecimal string with "0x" prefix
        r: string, // hexadecimal string with "0x" prefix
        s: string, // hexadecimal string with "0x" prefix
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



### Transaction Signing and Broadcasting example

```typescript
import type { UnsignedTransaction } from '@ethersproject/transactions';
import { TransactionTypes, serialize } from '@ethersproject/transactions';
import { keccak256 } from '@ethersproject/keccak256';
function buildSignedTxFromSignatureEvm({
  tx,
  signature,
}: {
  tx: UnsignedTransaction;
  signature: {
    v: string | number; // '0x11' , '17', 17
    r: string; // prefix 0x
    s: string; // prefix 0x
  };
}) {
  const { r, s, v } = signature;
  /**
   * sdk legacy return {v,r,s}; eip1559 return {recoveryParam,r,s}
   * splitSignature auto convert v to recoveryParam
   */
  const sig = splitSignature({
    v: Number(v),
    r,
    s,
  });
  const rawTx = serialize(tx, sig);
  const txid = keccak256(rawTx);
  return {
    rawTx,
    txid,
  };
}

// 1. Create transaction parameters
const txParams = {
    to: '0x7314e0f1c0e28474bdb6be3e2c3e0453255188f8',
    value: '0x0', // Default to 0 if no value
    data: '0x', // Empty data
    chainId: 1,
    nonce: '0x0',
    // For EIP-1559 transaction
    maxFeePerGas: '0x14', // 20
    maxPriorityFeePerGas: '0x0', // 0
    gasLimit: '0x5208', // 21000
    // For legacy transaction
    // gasPrice: '0x14', // 
};

// 2. Sign the transaction
const result = await HardwareSDK.evmSignTransaction(connectId, deviceId, {
    path: "m/44'/60'/0'",
    transaction: txParams,
    ...deviceCommonParams
});

// 3. Build signed transaction
const baseTx = {
    to: txParams.to,
    nonce: parseInt(txParams.nonce, 16),
    gasLimit: txParams.gasLimit
    data: txParams.data,
    value: txParams.value,
    chainId: txParams.chainId,
};

// Add EIP-1559 specific fields
const isEIP1559 = txParams?.maxFeePerGas || txParams?.maxPriorityFeePerGas;
if (isEIP1559) {
    Object.assign(baseTx, {
      type: TransactionTypes.eip1559,
      maxFeePerGas: txParams?.maxFeePerGas
      maxPriorityFeePerGas: txParams?.maxPriorityFeePerGas
    });
} else {
    Object.assign(baseTx, {
      gasPrice: txParams.gasPrice,
    });
}

// 4. Build final transaction with signature
const { rawTx, txid } = buildSignedTxFromSignatureEvm({
    tx: baseTx,
    signature: result.payload
});

// 5. Broadcast the transaction
// const provider = new ethers.providers.JsonRpcProvider();
// const broadcastedTx = await provider.sendTransaction(rawTx);

// 6. Wait for confirmation
// const receipt = await broadcastedTx.wait();
// console.log('Transaction confirmed:', receipt);
```

