# Firmware Updates

Update stm32 firmware / Bluetooth firmware version.\
\
Firmware upgrades are supported in two ways:

* Use the firmware upgrade resource on the OneKey server and simply pass in the version number.&#x20;
* Use the user-specified binary data to upgrade the firmware. Use this method with caution; incorrect data can result in hardware data loss.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.firmwareUpdate(connectId, params);
```
{% endtab %}
{% endtabs %}

## Attributes

| Name       | Instruction                                                                                                                                | Required | Type                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------ | -------- | --------------------- |
| connectId  | Connect ID                                                                                                                                 | Yes      | `String`              |
| params     | See the `params` type below for details                                                                                                    | Yes      | `Object`              |
| version    | Firmware version number. Specifying the firmware version number will match the version information in the resource list provided by OneKey | No       | `Array`               |
| updateType | Firmware upgrade type                                                                                                                      | No       | `'firmware' \| 'ble'` |
| binary     | Firmware binary data                                                                                                                       | No       | `ArrayBuffer`         |



## Params types

```typescript
export interface FirmwareUpdateBinary {
  binary: ArrayBuffer;
}

export interface FirmwareUpdate {
  version: number[];
  updateType: 'firmware' | 'ble';
}

export declare function firmwareUpdate(
  connectId: string | undefined,
  params: Params<FirmwareUpdate>
): Response<PROTO.Success>;
export declare function firmwareUpdate(
  connectId: string | undefined,
  params: Params<FirmwareUpdateBinary>
): Response<PROTO.Success>;

```

## Respond value

| Name         | Instruction                                                                                                                       | Type     |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `connectId`  | Device Connect ID                                                                                                                 | `String` |
| `uuid`       | Device Unique ID                                                                                                                  | `String` |
| `deviceId`   | Device ID, this ID may change with device erasure, helper change II. Only returned when using the `@onekeyfe/hd-web-sdk` library. | `String` |
| `deviceType` | Device Type                                                                                                                       | `String` |
| `name`       | Bluetooth name of the device                                                                                                      | `String` |

## Example



Respond value example:

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

