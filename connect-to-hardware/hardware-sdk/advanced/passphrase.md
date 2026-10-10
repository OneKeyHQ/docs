# Passphrase

A passphrase combines with a recovery phrase to derive a wallet. An empty passphrase selects the standard wallet; a different passphrase derives a different wallet, including its accounts and addresses.

A passphrase is sometimes called a “25th word”, but it is not restricted to a recovery-word list. Case, spaces, and symbols matter. A mistyped passphrase can open a different, empty wallet instead of returning a password error. Losing either the recovery phrase or the passphrase can prevent recovery of the corresponding wallet.

A passphrase does not make a compromised recovery phrase safe by itself: an attacker can try to guess a weak passphrase. Do not claim that the SDK permanently stores no passphrase data or clears every cache after each call; caching and device session lifetimes depend on the device, firmware, and SDK version.

## Select the standard wallet

Set `useEmptyPassphrase: true` explicitly when the user intends to use the standard wallet.

```typescript
const response = await HardwareSDK.btcGetAddress(connectId, deviceId, {
  path: "m/84'/0'/0'/0/0",
  coin: 'btc',
  showOnOneKey: true,
  useEmptyPassphrase: true,
});

if (response.success === false) {
  throw new Error(response.payload.error);
}
const address = response.payload.address;
```

This selects the empty-passphrase wallet. PIN entry and device confirmation may still be required.

## Handle a passphrase request

Register the listener before starting an operation. Choose a single response based on the user's wallet selection and the device's input capabilities.

```typescript
import { UI_EVENT, UI_REQUEST, UI_RESPONSE } from '@onekeyfe/hd-core';

HardwareSDK.on(UI_EVENT, (message) => {
  if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        value: '',
        passphraseOnDevice: true,
        save: false,
      },
    });
  }
});
```

The example requests on-device entry. For software entry, provide the user's passphrase in `value` and set `passphraseOnDevice: false`. Keep that value out of logs and persistent application storage. If the SDK supplies UI correlation fields, return them as described in [Handle SDK Events](../config-event.md).

## Continue with the same hidden wallet

The legacy `getPassphraseState` method requests the wallet state and may prompt for a passphrase. Check success and require a returned state before continuing a hidden-wallet operation.

```typescript
const stateResponse = await HardwareSDK.getPassphraseState(connectId);
if (stateResponse.success === false) {
  throw new Error(stateResponse.payload.error);
}
if (!stateResponse.payload) {
  throw new Error('No hidden-wallet state was returned');
}

const response = await HardwareSDK.btcGetAddress(connectId, deviceId, {
  path: "m/84'/0'/0'/0/0",
  coin: 'btc',
  showOnOneKey: true,
  passphraseState: stateResponse.payload,
});
if (response.success === false) {
  throw new Error(response.payload.error);
}
const address = response.payload.address;
```

Treat `passphraseState` as an opaque wallet selector. Do not log it, infer the passphrase from it, or assume it guarantees that the device remains unlocked. The SDK may ask for input again or reject a state mismatch after a session change. Do not silently retry a failed hidden-wallet operation with an empty passphrase.

Current SDK versions deprecate `getPassphraseState` in favor of [openWalletSession](https://developer.onekey.so/en/hardware-sdk/basic-api/open-wallet-session). Follow the API for the installed SDK version when migrating.
