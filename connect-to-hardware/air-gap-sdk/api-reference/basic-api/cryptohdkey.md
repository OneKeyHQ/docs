# CryptoHDKey

The `CryptoHDKey` class represents hierarchical deterministic key information

OneKey can export this registry object as a QR code so the wallet can derive addresses from public key data.

### Parameters

* `isMaster`: `boolean` Whether it is a master key.
* `isPrivateKey`: `boolean` Whether it is a private key.
* `key`: `Buffer` The key data.
* `chainCode`: `Buffer` The chain code.
* `useInfo`:[`CryptoCoinInfo`](cryptocoininfo.md) Usage information.
* `origin`: [`CryptoKeypath`](cryptokeypath.md) The origin path.
* `children`:[`CryptoKeypath`](cryptokeypath.md) The children path.
* `parentFingerprint`:`Buffer` The parent fingerprint.
* `name`: `string` The name. optional
* `note`:`Note(string)` The note. optional
  * 'account.standard' : BIP44 Standard account
  * "account.ledger\_live" : Ledger Live account
  * "account.ledger\_legacy" : Ledger Legacy account

### URL Example

{% code overflow="wrap" %}
```
UR:CRYPTO-HDKEY/PDAXHDCLAOZTRDKBTKFPRFKBCWVEWYBGDPNTCPVLEOENJSWMBKFTLTRESNWTNLTLMKJYVYMWBSAAHDCXCSBNNLLNBZIAJZTPKPPKJOSTCEZSJEKGYKJOCSKNHFTPSWTIGHVABDIEGTBWWLTEAHTAADEHOYADCSFNAMTAADDYOYADLNCSDWYKCSFNYKAEYKATTAADDYOYADLRAEWKLAWKAYAEASINFPIAIAJLKPJTJYCXEHBKKOGHISINJKCXINJKCXHSC
```
{% endcode %}

## Decode account data

```typescript
import { URDecoder, CryptoHDKey } from '@onekeyfe/hd-air-gap-sdk';

const decoder = new URDecoder();

function onAccountFrame(frame: string) {
  decoder.receivePart(frame);
  if (decoder.isError()) throw new Error('Could not decode account QR code');
  if (!decoder.isSuccess()) return;

  const ur = decoder.resultUR();
  if (ur.type !== 'crypto-hdkey') throw new Error('Expected crypto-hdkey');
  const hdKey = CryptoHDKey.fromCBOR(ur.cbor);
  if (hdKey.isPrivateKey()) throw new Error('Expected a public account export');

  const origin = hdKey.getOrigin();
  const originPath = origin?.getPath();
  const fingerprint = origin?.getSourceFingerprint();
  if (!originPath || !fingerprint) {
    throw new Error('Account export is missing its origin or fingerprint');
  }
  return {
    xpub: hdKey.getBip32Key(),
    originPath: `m/${originPath}`,
    masterFingerprintHex: fingerprint.toString('hex'),
    childrenPath: hdKey.getChildren()?.getPath(),
    name: hdKey.getName(),
    note: hdKey.getNote(),
  };
}
```

Use the exported origin and children path when deriving addresses. An absent children path is not proof of a particular account layout; handle it according to the wallet's supported derivation scheme. See the [wallet integration tutorial](../../tutorial-wallet-integration.md).
