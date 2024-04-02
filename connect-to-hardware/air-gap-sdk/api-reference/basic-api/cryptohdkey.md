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
* `note`:`Note(string)`  The note. optional
  * 'account.standard' : BIP44 Standard account
  * "account.ledger\_live" : Ledger Live account
  * "account.ledger\_legacy" : Ledger Legacy account



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
    
    const name = cryptoHDKey.getName();
    const note = cryptoHDKey.getNote();
    
    const extendPubKey = hdKey.getKey();
    const chainCode = hdKey.getChainCode();
    
    const xpub = hdKey.getBip32Key();
    const childrenPath = hdKey.getChildren()?.getPath() ?? DEFAULT_CHILDREN_PATH;
    const hdPath = `m/${cryptoHDKey.getOrigin().getPath()}`;
    // This parameter is required for subsequent eth sign request assembly.
    const xfp = cryptoHDKey.getOrigin().getSourceFingerprint()?.toString("hex");
    
    // derive child
    const accountIndex = 0
    const derivePath = childrenPath
            .replace("*", String(accountIndex))
            .replace(/\*/g, "0");
    
    const hdk = HDKey.fromExtendedKey(xpub);
    const dkey = hdk.derive(`m/${derivePath}`);
    const address =
            "0x" + publicToAddress(dkey.publicKey, true).toString("hex");
    const addressWithChecksum = toChecksumAddress(address);
} else if(decoder.isError()){
    // logic for error handling
    throw new Error() 
}
```



