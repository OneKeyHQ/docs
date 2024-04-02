# CryptoHDkey

The `CryptoHDKey` class represents hierarchical deterministic key information

This is an instruction provided by the OneKey hardware, which includes the extened public key information.

### Parameters

* `isMaster`: `boolean` Whether it is a master key.
* `isPrivateKey`: `boolean`  Whether it is a private key.
* `key`: `Buffer`  The key data.
* `chainCode`: `Buffer` The chain code.
* `useInfo`:[`CryptoCoinInfo`](cryptocoininfo.md)  Usage information.
* `origin`: [`CryptoKeypath`](cryptokeypath.md) The origin path.
* `children`:[`CryptoKeypath`](cryptokeypath.md)  The children path.
* `parentFingerprint`:`Buffer`  The parent fingerprint.
* `name`: `string`  The name. optional
* `note`:`string`  The note. optional



### URL Example

{% code overflow="wrap" %}
```
UR:CRYPTO-HDKEY/PDAXHDCLAOZTRDKBTKFPRFKBCWVEWYBGDPNTCPVLEOENJSWMBKFTLTRESNWTNLTLMKJYVYMWBSAAHDCXCSBNNLLNBZIAJZTPKPPKJOSTCEZSJEKGYKJOCSKNHFTPSWTIGHVABDIEGTBWWLTEAHTAADEHOYADCSFNAMTAADDYOYADLNCSDWYKCSFNYKAEYKATTAADDYOYADLRAEWKLAWKAYAEASINFPIAIAJLKPJTJYCXEHBKKOGHISINJKCXINJKCXHSC
```
{% endcode %}



## Example

```javascript
import { URRegistryDecoder } from '@onekeyfe/hd-air-gap-sdk'

const decoder = new URRegistryDecoder();

// for scan qr
while (!decoder.isSuccess()){
    const UR = ScanQRCode();
    decoder.receivePart(UR);
}

if(decoder.isSuccess()) {
    const cryptoHDkey = decoder.resultRegistryType();
    const path = cryptoHDkey.getOrigin().getPath()); // m/44'/60'/0'
    const extenedPubKey = cryptoHDKey.getBip32Key(); // xpub
    return
} else if(decoder.isError()){
    // logic for error handling
    throw new Error() 
}
```



