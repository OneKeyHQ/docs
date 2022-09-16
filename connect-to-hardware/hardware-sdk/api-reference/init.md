# Initialization

Initialize the SDK and returns the initialization result to the caller.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const result = await HardwareSDK.init(params);
```
{% endtab %}
{% endtabs %}

## Parameters

| Attributes   | Description                                                                                                                                                                        | Required                                                                                | Type                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------- |
| `debug`      | Console log output switch                                                                                                                                                          | No                                                                                      | `Boolean`                 |
| `connectSrc` | The `url` of the `iframe` page, when using the `@onekeyfe/hd-web-sdk` web connection library, needs to be passed in the path of the `iframe` in order to establish the connection. | Required when using `@onekeyfe/hd-web-sdk`, not required for the rest of the libraries. | `String`                  |
| `env`        | The environment to use the SDK, optional parameter, if not passed the SDK detects the calling environment itself.                                                                  | No                                                                                      | `'web' \| 'react-native'` |
