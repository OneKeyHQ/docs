# commonParams

Every call require an [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Object) with combination of common and method specified fields. All common parameters are optional.

*   `device` - _optional_ `Object`

    * `path` - _required_ `string` call to a direct device. Useful when working with multiple connected devices. This value is emitted by `OneKeyConnectEvent`
    * `state` - _optional_ `string` sets expected state. This value is emitted by `OneKeyConnectEvent`
    * `instance` - _optional_ `number` sets an instance of device. Useful when working with one device and multiple passphrases. This value is emitted by `OneKeyConnectEvent`

    ``
* `useEmptyPassphrase` — _optional_ `boolean` method will not ask for a passphrase. Default is set to `false`

``

* `allowSeedlessDevice` — _optional_ `boolean` allows to use OneKeyConnect methods with device with seedless setup. Default is set to `false`

``

* `keepSession` — `optional boolean` Advanced feature. After method return a response device session will NOT! be released. Session shoul be released after all calls are performed by calling any method with `keepSession` set to false or `undefined`.&#x20;
* Useful when you need to do multiple different calls to OneKeyConnect API without releasing. Example sequence loop for 10 account should look like:
  * `OneKeyConnect.getPublicKey({ device: { path: "web01"}, keepSession: true, ...otherParams })` for first account,
  * `OneKey.getAddress({ device: { path: "web01"}, ...otherParams })` for the same account,
  * looking up for balance in external blockchain
  * loop iteration
  * after last iteration call `OneKeyConnect.getFeatures({ device: { path: "web01"}, keepSession: false, ...otherParams })`
