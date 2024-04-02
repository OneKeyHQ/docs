# EthSignRequest

The `EthSignRequest` class handles Ethereum signing requests.

Used to send a signing request to the device.



```
enum DataType {
    transaction = 1, // For the legacy transaction, the rlp encoding of the unsigned data.
    typedData = 2, // For the EIP-712 typed data. Bytes of the json string.
    personalMessage = 3, // For the personal message signing. 
    typedTransaction = 4 // For the typed transaction, like the EIP-1559 transaction.
}
```

### Parameters

* `requestId`: `Buffer` The request ID.  uuid random
* `signData`: `Buffer` The data to be signed.
* `dataType`: `DataType` The type of signing data.
* `chainId`: `number` The chain ID. optional
* `derivationPath`: [`CryptoKeypath`](../basic-api/cryptokeypath.md) The derivation path.
* `address`: `Buffer` The address for request this signing. optional
* `origin`: `string` Source of the request. optional



### URL Example

{% code overflow="wrap" %}
```
UR:ETH-SIGN-REQUEST/ONADTPDAGDSWNNYAHGTOKPFPIAPANNROLNSAVYDTHHAOHDECAOWFLYLDLFAAUELPATAEGWSOLALPBAVYGUYTHNLFGMAYMWSGBYZOIYHTRDCFBANBFPBNDSPRJPNSDLGLBYIMJELTCNLNWZJLSEAEAELARTAXAAAACSLDAHTAADDYOEADLECSDWYKCSFNYKAEYKAEWKADWKAOCYTIZSYLCNSSDKGOCA
```
{% endcode %}

### **EIP1559 Transaction**

```javascript
import { Transaction, FeeMarketEIP1559Transaction } from '@ethereumjs/tx';
import Common, { Hardfork } from '@ethereumjs/common';
import { BN } from 'ethereumjs-util';
import {
    CryptoHDKey,
    EthSignRequest,
    DataType,
    ETHSignature,
} from '@onekeyfe/hd-air-gap-sdk';

const common = Common.forCustomChain('mainnet', { chainId: this._networkId }, Hardfork.London);
const eip1559Tx = FeeMarketEIP1559Transaction.fromTxData(txParams, { common });
const unsignedBuffer = Buffer.from(eip1559Tx.getMessageToSign(false)); // generate the unsigned transaction bytes
const requestId = randomUUID();
const addressPath = "m/44'/60'/0'/0/0"

const ethSignRequest = EthSignRequest.constructETHRequest(
    unsignedBuffer,
    DataType.typedTransaction,
    addressPath,
    "12345678", // master fingerprint
    requestId,
    1, // chainId
    "from address",
);

// each chunk Number in single QR Code
const maxChunkNumber = 200;

// get the ur encoder
const urEncoder = ethSignRequest.toUREncoder(maxChunkNumber);

while (ture) {
    delay(200);
    renderQR(urEncoder.nextPart());
}

```

### **Legacy Transaction**

```javascript
import { Transaction } from '@ethereumjs/tx';
import Common, { Hardfork } from '@ethereumjs/common';
import { BN } from 'ethereumjs-util';
import {
    CryptoHDKey,
    EthSignRequest,
    DataType,
    ETHSignature,
} from '@onekeyfe/hd-air-gap-sdk';

const _txParams = {
    to: "0x0102030405", 
    gasLimit: 200000,
    gasPrice: 120000000000,
    data: "0x",
    nonce: 1,
    value: txParams.value,
};

const tx = Transaction.fromTxData(_txParams, { common: this._common, freeze: false });
        
tx.v = new BN(tx.common.chainId());
tx.r = new BN(0);
tx.s = new BN(0);

const unsignedBuffer = tx.serialize(); // generate the unsigned transaction bytes
const requestId = randomUUID();
const addressPath = "m/44'/60'/0'/0/0"

const ethSignRequest = EthSignRequest.constructETHRequest(
    unsignedBuffer,
    DataType.transaction,
    addressPath,
    "12345678", // master fingerprint
    requestId,
    1, // chainId
    "from address",
);

// each chunk Number in single QR Code
const maxChunkNumber = 200;

// get the ur encoder
const urEncoder = ethSignRequest.toUREncoder(maxChunkNumber);

while (ture) {
    delay(200);
    renderQR(urEncoder.nextPart());
}

```

### **EIP-712 TypedData**

```javascript
import {
    CryptoHDKey,
    EthSignRequest,
    DataType,
    ETHSignature,
} from '@onekeyfe/hd-air-gap-sdk';

const typeData = "{}"
const dataHex = Buffer.from(typedData, 'utf-8');
const requestId = randomUUID();
const addressPath = "m/44'/60'/0'/0/0"

const ethSignRequest = EthSignRequest.constructETHRequest(
    dataHex,
    DataType.typedData,
    addressPath,
    "12345678", // master fingerprint
    requestId,
    undefined,
    "from address",
);

// each chunk Number in single QR Code
const maxChunkNumber = 200;

// get the ur encoder
const urEncoder = ethSignRequest.toUREncoder(maxChunkNumber);

while (ture) {
    delay(200);
    renderQR(urEncoder.nextPart());
}

```

### **Message Sign**

```javascript
import {
    CryptoHDKey,
    EthSignRequest,
    DataType,
    ETHSignature,
} from '@onekeyfe/hd-air-gap-sdk';

const message = "0102030405"
const dataHex = Buffer.from(message, 'hex');
const requestId = randomUUID();
const addressPath = "m/44'/60'/0'/0/0"

const ethSignRequest = EthSignRequest.constructETHRequest(
    dataHex,
    DataType.typedData,
    addressPath,
    "12345678", // master fingerprint
    requestId,
    undefined,
    "from address",
);

// each chunk Number in single QR Code
const maxChunkNumber = 200;

// get the ur encoder
const urEncoder = ethSignRequest.toUREncoder(maxChunkNumber);

while (ture) {
    delay(200);
    renderQR(urEncoder.nextPart());
}

```



Obtaining a signature requires viewing.

{% content-ref url="ethsignature.md" %}
[ethsignature.md](ethsignature.md)
{% endcontent-ref %}
