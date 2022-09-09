# Check for Bluetooth Firmware Updates

Check the current Bluetooth firmware version status of the device.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.checkBLEFirmwareRelease(connectId);
```
{% endtab %}
{% endtabs %}

## Attributes

| Name      | Instruction | Required | Type |
| --------- | ----------- | -------- | ---- |
| connectId | Connect ID  | Yes      |      |

## Respond value

| Name        | Instruction                | Type                                                         |
| ----------- | -------------------------- | ------------------------------------------------------------ |
| `status`    | Current Firmware Status    | `'valid' \| 'outdated' \| 'required' \| 'unknown' \| 'none'` |
| `changelog` | Update Logs                | `Object`                                                     |
| `release`   | Latest version information | `Object`                                                     |

## Example

Respond value example：

```typescript
{
  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 9,
  "success": true,
  "payload": {
    "status": "valid",
    "changelog": [],
    "release": {
      "required": false,
      "version": [
        1,
        2,
        1
      ],
      "url": "https://onekey-asset.com/onekey/ble/v1.2.1/App_Signed-2021-4-1_1.2.1.zip",
      "webUpdate": "https://onekey-asset.com/onekey/ble/v1.2.1/App_Signed-2021-4-1_1.2.1.bin",
      "fingerprint": "fbcb149427dd74c3fba48bcbe55799168f252a4e08c053aa2b98c78fba6ef8f7",
      "fingerprintWeb": "fbcb149427dd74c3fba48bcbe55799168f252a4e08c053aa2b98c78fba6ef8f7",
      "changelog": {
        "zh-CN": "修复已知问题",
        "en-US": "minor fixes"
      }
    }
  }
}
```
