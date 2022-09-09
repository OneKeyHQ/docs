# Get Device Information

Get device details.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.getFeatures(connectId);
```
{% endtab %}
{% endtabs %}

## Attributes

| Name        | Instruction | Required | Type |
| ----------- | ----------- | -------- | ---- |
| `connectId` | Connect ID  | Yes      |      |

## Respond value

| Name                    | Instruction                  | Type      |
| ----------------------- | ---------------------------- | --------- |
| `initialized`           | Device initialization status | `Boolean` |
| `ble_name`              | Bluetooth Name               | `String`  |
| `ble_ver`               | Bluetooth firmware version   | `String`  |
| `bootloader_version`    | bootloader version           | `String`  |
| `deviceId`              | Device ID                    | `String`  |
| `language`              | Language                     | `String`  |
| `label`                 | Equipment Name               | `String`  |
| `major_version`         | Major version number         | `Number`  |
| `minor_version`         | Middle version number        | `Number`  |
| `patch_version`         | Patch version number         | `Number`  |
| `onekey_serial`         | Device Serial Number         | `String`  |
| `onekey_version`        | Firmware Version             | `String`  |
| `passphrase_protection` | Whether enable passphrase    | `Boolean` |



## Example

Respond value example：

```typescript
{
  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 5,
  "success": true,
  "payload": {
    "major_version": 1,
    "minor_version": 99,
    "patch_version": 99,
    "bootloader_mode": null,
    "device_id": "0B961C0007C7723D5E5E3361",
    "pin_protection": true,
    "passphrase_protection": false,
    "language": "zh-CN",
    "label": "EA2AA",
    "initialized": true,
    "model": "1",
    "safety_checks": "Strict",
    "auto_lock_delay_ms": null,
    "display_rotation": null,
    "experimental_features": null,
    "offset": null,
    "ble_name": "K4521",
    "ble_ver": "1.2.1",
    "ble_enable": true,
    "se_enable": false,
    "se_ver": "1.1.0.3",
    "backup_only": false,
    "onekey_version": "2.3.0",
    "onekey_serial": "OneKey1042003983",
    "bootloader_version": "1.8.8",
  }
}
```
