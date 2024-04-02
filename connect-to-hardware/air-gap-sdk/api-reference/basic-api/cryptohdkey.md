# CryptoHDkey

The `CryptoHDKey` class represents hierarchical deterministic key information

This is an instruction provided by the OneKey hardware, which includes the public key information.

### Parameters

* `isMaster`: Whether it is a master key.
* `isPrivateKey`: Whether it is a private key.
* `key`: The key data.
* `chainCode`: The chain code.
* `useInfo`: Usage information, of type [`CryptoCoinInfo`](cryptocoininfo.md).
* `origin`: The origin path, of type [`CryptoKeypath`](cryptokeypath.md).
* `children`: The children path, of type [`CryptoKeypath`](cryptokeypath.md).
* `parentFingerprint`: The parent fingerprint.
* `name`: The name.
* `note`: The note.



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



