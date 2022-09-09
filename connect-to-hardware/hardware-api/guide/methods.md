# Methods

API call return a [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise). Resolve is guaranteed to get called with a `result` object, even if user closes the window, network connection times out, etc. In case of failure `result.success` is set to false and `result.payload.error` is the error message. It is recommended to log the error message and let user restart the action.

Every method require an [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Object) with combination of `common` fields and method specific fields.

* OneKeyConnect.getPublicKey
* OneKeyConnect.requestLogin
* OneKeyConnect.cipherKeyValue
* OneKeyConnect.wipeDevice
* OneKeyConnect.resetDevice
* OneKeyConnect.getCoinInfo
* OneKeyConnect.bixinReboot

### Bitcoin, Bitcoin Cash, Bitcoin Gold, Litecoin, Dash, ZCash, Testnet

* OneKeyConnect.getAddress
* OneKeyConnect.getAccountInfo
* OneKeyConnect.composeTransaction
* OneKeyConnect.signTransaction
* OneKeyConnect.pushTransaction
* OneKeyConnect.signMessage
* OneKeyConnect.verifyMessage

### Ethereum

* OneKeyConnect.ethereumGetAddress
* OneKeyConnect.ethereumSignTransaction
* OneKeyConnect.ethereumSignMessage
* OneKeyConnect.ethereumVerifyMessage

### Eos

* OneKeyConnect.eosGetPublicKey
* OneKeyConnect.eosSignTransaction

### NEM

* OneKeyConnect.nemGetAddress
* OneKeyConnect.nemSignTransaction

### Stellar

* OneKeyConnect.stellarGetAddress
* OneKeyConnect.stellarSignTransaction

### Lisk

* OneKeyConnect.liskGetAddress
* OneKeyConnect.liskSignMessage
* OneKeyConnect.liskVerifyMessage
* OneKeyConnect.liskSignTransaction
