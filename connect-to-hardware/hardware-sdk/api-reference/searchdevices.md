# Searching Devices

Searches for connected devices and returns the search results to the developer as an array. \
\
In the case of USB connections, the returned data already contains device details. \
\
In the case of Bluetooth device search, the data returned contains only the device name and device connectId, and the developer selects the device that needs to be paired before getting the device information.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.searchDevice();
```
{% endtab %}
{% endtabs %}

## Attributes

| Name | Instructions | Required | Type |
| ---- | ------------ | -------- | ---- |
|      |              |          |      |

## Respond

| Name         | Instructions                                                                                                                      | Type     |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `connectId`  | Device Connection ID                                                                                                              | `String` |
| `uuid`       | Device Unique ID                                                                                                                  | `String` |
| `deviceId`   | Device ID, this ID may change with device erasure, helper change II. Only returned when using the `@onekeyfe/hd-web-sdk` library. | `String` |
| `deviceType` | Device Type                                                                                                                       | `String` |
| `name`       | Bluetooth name for the device                                                                                                     | `String` |

## Example

Example of return value.

<pre class="language-javascript"><code class="lang-javascript"><strong>{
</strong>  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 3,
  "success": true,
  "payload": [
    {
      "connectId": "OneKey21042004483",
      "uuid": "OneKey21042004483",
      "deviceType": "classic",
      "deviceId": "0B961C0098C7923D5E5E3361",
      "path": "1",
      "name": "K4521"
    },
    {
      "connectId": "MI05W01202112319902230004357",
      "uuid": "MI05W01202112319902230004357",
      "deviceType": "mini",
      "deviceId": "5901442ED2C36B1F4E8DA991",
      "path": "2",
      "name": "My Mini"
    }
  ]
}</code></pre>

