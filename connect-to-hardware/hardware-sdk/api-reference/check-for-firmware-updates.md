# Check for Firmware Updates

Check the current firmware version status of the device.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.checkFirmwareRelease(connectId);
```
{% endtab %}
{% endtabs %}

## Attributes

| Name      | Instruction | Required | Type |
| --------- | ----------- | -------- | ---- |
| connectId | Connect ID  | Yes      |      |

## Respond Value

| Name        | Instruction                | Type                                                         |
| ----------- | -------------------------- | ------------------------------------------------------------ |
| `status`    | Current Firmware Status    | `'valid' \| 'outdated' \| 'required' \| 'unknown' \| 'none'` |
| `changelog` | Update Logs                | `Object`                                                     |
| `release`   | Latest version information | `Object`                                                     |



## Example

Respond Value Example：

```javascript
{
  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 8,
  "success": true,
  "payload": {
    "status": "valid",
    "changelog": [],
    "release": {
      "required": false,
      "version": [
        2,
        2,
        0
      ],
      "url": "https://onekey-asset.com/onekey/hw/v2.2.0/classic.2.2.0-[Stable-0507-1].bin",
      "fingerprint": "fbcb149427dd74c3fba48bcbe55799168f252a4e08c053aa2b98c78fba6ef8f7",
      "changelog": {
        "zh-CN": "支持 signTypedData 方法",
        "en-US": "support signTypedData method"
      }
    }
  }
}
```
