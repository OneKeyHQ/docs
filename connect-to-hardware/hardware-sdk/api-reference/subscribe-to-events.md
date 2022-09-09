# Subscribe to Events

Subscribe to the SDK's events.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
HardwareSDK.on(event, callback);
```
{% endtab %}
{% endtabs %}

## Attributes

| Name     | Instruction                                    | Required | Type       |
| -------- | ---------------------------------------------- | -------- | ---------- |
| event    | Event type, refer to [Event](broken-reference) | Yes      | `String`   |
| callback | Callback functions                             | Yes      | `Function` |
