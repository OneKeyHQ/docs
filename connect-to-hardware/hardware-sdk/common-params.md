# Common Params

In the SDK method calls, there are three generic parameters: `connectId` , `deviceId` , `commonParams`.

```typescript
function call(connectId: string; deviceId: string; commonParams: CommonParams)
```

The input parameters are obtained as follows:

* `connectId`: Obtained from the `connectId` field of the device returned in the `searchDevice` interface.&#x20;
  * The connect id of the device is never changed.
* `deviceId`: The deviceId field returned by the `getFeatures` interface.&#x20;
  * The device id changes when the hardware is reset. Like wipe the device.
* `commonParams`: common parameters&#x20;
  * `retryCount`: optional `number` type. The number of retries when the device is connected, default is 6.&#x20;
  * `pollIntervalTime`: optional `number` type. The interval time for polling when the device is connected, default is 1000 ms, each polling will increase the time by 1.5 times.&#x20;
  * `timeout`: optional `number` type. The timeout of the connection polling.
  * `keepSession`: optional `boolean` type. The Session persists after executing the API method.
  * `passphraseState`: optional `string` type. If you want to use a passphrase wallet, please pass that parameter. We will validate and cache the passphrase to optimize the user experience of entering the passphrase and reduce the number of input attempts. To retrieve that parameter, please call the `getPassphraseState` method.
  * `useEmptyPassphrase`: optional `boolean` type. Allow the creation of a passphrase wallet with an empty value.
  * `initSession` : optional `boolean` type. Cache the passphraseState parameter.
  * `deriveCardano`: optional `boolean` type. default is set to `true` for all cardano related methods, otherwise it is set to `false`. This parameter determines whether device should derive cardano seed for current session. Derivation of cardano seed takes longer then it does for other coins.
