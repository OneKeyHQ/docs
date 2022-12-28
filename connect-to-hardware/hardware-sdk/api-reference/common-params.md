# Common Params

In the SDK method calls, there are three generic parameters: `connectId` , `deviceId` , `commonParams`.

```typescript
function call(connectId: string; deviceId: string; commonParams: CommonParams)
```

The input parameters are obtained as follows:

* `connectId`: Obtained from the `connectId` field of the device returned in the `searchDevice` interface.&#x20;
* `deviceId`: The deviceId field returned by the `getFeatures` interface.&#x20;
* `commonParams`: common parameters&#x20;
  * `retryCount`: optional `number` type. The number of retries when the device is connected, default is 6.&#x20;
  * `pollIntervalTime`: optional `number` type. The interval time for polling when the device is connected, default is 1000 ms, each polling will increase the time by 1.5 times.&#x20;
  * `timeout`: optional `number` type. The timeout of the connection polling.
