# Check Bridge Updates

Check the version update status of the hardware connection tool `Bridge`.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.checkTransportRelease();
```
{% endtab %}
{% endtabs %}

## Attributes

| Name | Instruction | Required | Type |
| ---- | ----------- | -------- | ---- |
|      |             |          |      |

## Respond value

| Name      | Instruction                   | Type                    |
| --------- | ----------------------------- | ----------------------- |
| `payload` | Local `Bridge` Version Status | `'valid' \| 'outdated'` |



## Example

Respond value example：

```typescript
{
  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 11,
  "success": true,
  "payload": "valid"
}
```
