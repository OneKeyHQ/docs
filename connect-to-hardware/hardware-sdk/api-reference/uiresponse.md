# uiResponse

### Transfer data to the device

Transfer data to the device, currently supporting data transfer for PIN and passphrase.

<pre class="language-typescript"><code class="lang-typescript"><strong>const response = await HardwareSDK.uiResponse(response);
</strong></code></pre>

### Params

[**Optional common params**](common-params.md)

#### Return the PIN code result

```typescript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PIN,
  payload: ${pin_value},
});
```

* If entering the PIN on the software side, replace the content of `pin_value`. If the PIN needs to be entered on the hardware side, set `pin_value` as `@@ONEKEY_INPUT_PIN_IN_DEVICE`.

#### Return the Passphrase result

```
SDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PASSPHRASE,
  payload: {
    value: ${passphrase_value},
    passphraseOnDevice: false,
    save: false,
  },
});
```

* If the passphrase needs to be entered on the device, set value to '' and set passphraseOnDevice to true.
