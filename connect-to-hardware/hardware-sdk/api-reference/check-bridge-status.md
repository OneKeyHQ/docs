# Check Bridge Status

Check the operational status of the hardware connection tool `Bridge`.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.checkBridgeStatus();
```
{% endtab %}
{% endtabs %}

## Attributes

| Name | Instruction | Required | Type |
| ---- | ----------- | -------- | ---- |
|      |             |          |      |

## Respond value

| Name      | Instruction     | Type      |
| --------- | --------------- | --------- |
| `payload` | `Bridge` Status | `Boolean` |

## Example

Respond value example：

```typescript
{
  "event": "RESPONSE_EVENT",
  "type": "RESPONSE_EVENT",
  "id": 13,
  "success": true,
  "payload": true
}
```
