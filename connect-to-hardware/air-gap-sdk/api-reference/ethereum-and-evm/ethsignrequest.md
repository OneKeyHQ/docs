# EthSignRequest

`EthSignRequest` encodes Ethereum signing data as a UR for the device to scan. It does not build a transaction or broadcast it.

## Data types

| Value | Name                        | Signing data                                                                                  |
| ----- | --------------------------- | --------------------------------------------------------------------------------------------- |
| 1     | `DataType.transaction`      | Unsigned legacy transaction encoded for signing, including chain protection where applicable. |
| 2     | `DataType.typedData`        | UTF-8 bytes of the complete EIP-712 JSON object.                                              |
| 3     | `DataType.personalMessage`  | Original message bytes for personal message signing.                                          |
| 4     | `DataType.typedTransaction` | Unsigned typed-transaction bytes, including the transaction type prefix.                      |

## Parameters

The constructor accepts `signData: Buffer`, `dataType: DataType`, and `derivationPath: CryptoKeypath`. Optional fields are `requestId: Buffer`, `chainId: number`, `address: Buffer`, and `origin: string`.

The `constructETHRequest` helper below instead accepts a path string, a master fingerprint as hex, and a UUID string. Obtain the path and fingerprint from the account exported by the device. Use a fresh UUID for each request and retain it until the response is verified.

## Build requests

```typescript
import { EthSignRequest, DataType } from '@onekeyfe/hd-air-gap-sdk';

function createRequest(
  signData: Buffer,
  dataType: DataType,
  addressPath: string,
  masterFingerprintHex: string,
  chainId?: number,
) {
  const requestId = globalThis.crypto.randomUUID();
  const request = EthSignRequest.constructETHRequest(
    signData,
    dataType,
    addressPath,
    masterFingerprintHex,
    requestId,
    chainId,
    undefined, // Optional signer address, as hex.
    'your-app',
  );
  return { requestId, request };
}
```

### EIP-1559 transaction

Use your Ethereum transaction library to obtain the unsigned, serialized EIP-1559 signing bytes. Include the `0x02` transaction type byte; do not pass a transaction hash or an already signed transaction.

```typescript
const pending = createRequest(
  unsignedEip1559Bytes,
  DataType.typedTransaction,
  addressPath,
  masterFingerprintHex,
  chainId,
);
```

### Legacy transaction

Pass the unsigned legacy signing payload produced by your transaction library for the intended chain. Do not construct the payload by filling signature fields with arbitrary values.

```typescript
const pending = createRequest(
  unsignedLegacyBytes,
  DataType.transaction,
  addressPath,
  masterFingerprintHex,
  chainId,
);
```

### EIP-712 typed data

`typedData` must contain the full `types`, `primaryType`, `domain`, and `message` objects. Encode integer values that exceed JavaScript's safe range as strings.

```typescript
const pending = createRequest(
  Buffer.from(JSON.stringify(typedData), 'utf8'),
  DataType.typedData,
  addressPath,
  masterFingerprintHex,
  chainId,
);
```

### Personal message

```typescript
const pending = createRequest(
  Buffer.from('Hello OneKey', 'utf8'),
  DataType.personalMessage,
  addressPath,
  masterFingerprintHex,
  chainId,
);
```

## Display animated QR frames

`200` is the encoder's maximum fragment length, not a count of QR frames. `renderQR` is supplied by your UI. Stop the timer when the dialog closes or the request completes.

```typescript
const encoder = pending.request.toUREncoder(200);
renderQR(encoder.nextPart());
const timer = setInterval(() => renderQR(encoder.nextPart()), 200);
// On completion or cancellation:
// clearInterval(timer);
```

The snippets share the helper above. Transaction bytes, account metadata, `chainId`, and `renderQR` come from your application. Decode the returned [ETHSignature](ethsignature.md) and verify that it belongs to this request before attaching it to the original transaction.
