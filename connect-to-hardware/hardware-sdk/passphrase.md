# Passphrase

If the hidden wallet is enabled, the `passphraseState` parameter needs to be passed in API calls as a [common parameter](api-reference/common-params.md). `passphraseState` is the identifier for the hidden wallet, which can be got by calling the [`getPassphraseState`](api-reference/getpassphrasestate.md) API.



For the same hidden wallet, the value of `passphraseState` remains the same. Therefore, you can persist the `passphraseState` to pass it in subsequent API calls.



The hardware temporarily caches the passphrase internally, but the cache may become invalid when the screen is locked, turned off, or when the cache limit is reached. If the cache is valid, passphrase does not need to be entered again in subsequent API calls. If the cache is not present, the passphrase needs to be entered again.



You can know when you need to enter a passphrase by listening to the “`ui-request_passphrase`” event.

### Examples of passphrase

#### The complete passphrase process

<pre class="language-typescript"><code class="lang-typescript"><strong>// Listening to UI events
</strong><strong>HardwareSDK.on(UI_EVENT, (message) => {
</strong>  if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    // Enter the passphrase on the device
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

/**
 * Check if the hidden wallet is enabled. 
 * If the return value is a string, it means the hidden wallet is enabled. 
 * If it is undefined, then the hidden wallet is not enabled.
 */
const passphraseStateRes = await HardwareSDK.getPassphraseState(connectId);
const params = {
	path: "m/84'/0'/0'/0/0",
	coin: "btc",
	showOnOneKey: true
}
// If the passphrase is enabled, you need to pass the ‘passphraseState’ parameter.
passphraseStateRes.payload &#x26;&#x26; (params['passphraseState'] = passphraseStateRes.payload)

// Finally, go and get your Bitcoin address
const response = await HardwareSDK.btcGetAddress(connectId, deviceId, params)
console.log('bitcoin address: ', response)
</code></pre>

#### Enter the passphrase in the software

You can prompt the user to quickly input the passphrase by popping up an input box after the UI event trigger of passphrase, which can enhance user experience.

You only need to return the result of passphrase through the uiResponse API like this.

```typescript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PASSPHRASE,
    payload: {
    value: '${user's passphrase}',
    passphraseOnDevice: true,
    save: true,
   },
});
```
