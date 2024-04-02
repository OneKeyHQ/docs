# EthSignature

The `ETHSignature` class represents an Ethereum signature.

Used to parse the signed result returned by the device.



#### Parameters

* `signature`: `Buffer` The signature data.
* `requestId`: `Buffer` The request ID, optional.
* `origin`: `string` The origin information, optional.



### URL Example

{% code overflow="wrap" %}
```
UR:ETH-SIGNATURE/OTADTPDAGDSWNNYAHGTOKPFPIAPANNROLNSAVYDTHHAOHDFPCATKCPPFYLENGAGLMKMUCAYKFPFSDREOMENTPKBGEONDCHFDNBKOSSTPDWETSGBZDNCMCHKPNYDMKIDPTDRYJSDRTKCTIOFPQZHFLNSKVACLMNIYTKLGISFRWLKTZTREAEAXIYGWJTIHGRIHKKPLGDEEDS
```
{% endcode %}



### Example

```javascript
import {
    ETHSignature,
    URDecoder,
    RegistryTypes,
} from '@onekeyfe/hd-air-gap-sdk';

const decoder = new URDecoder();

// for scan qr
while (!decoder.isSuccess()){
    const UR = ScanQRCode();
    decoder.receivePart(UR);
}

if(decoder.isSuccess()) {
    const ur = decoder.resultUR();
    if (!ur.type === RegistryTypes.ETH_SIGNATURE) throw new Error()
    
    const ethSignature = ETHSignature.fromCBOR(ur.cbor);
    const requestIdBuffer = ethSignature.getRequestId();
    const signature = ethSignature.getSignature();
    const r = signature.slice(0, 32);
    const s = signature.slice(32, 64);
    const v = signature.slice(64, 65);
} else if(decoder.isError()){
    // logic for error handling
    throw new Error() 
}
```
