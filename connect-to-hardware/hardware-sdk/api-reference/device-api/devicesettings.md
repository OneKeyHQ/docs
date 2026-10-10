# deviceSettings

## Device settings

It is recommended to modify only one property when using it, as it needs to be confirmed on the device.

```typescript
const result = await HardwareSDK.deviceSettings(connectId, params);
```

## Params

[**Optional common params**](../common-params.md)

* `language` — _optional_ `string` Device language, [ISO 639-1 codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
* `label` — _optional_ `string` Device name
* `usePassphrase` — _optional_ `boolean` Enable or disable passphrase support
* `homescreen` — _optional_ `string` Change homescreen by hex string
* `autoLockDelayMs` — _optional_ `number` Auto lock time in ms
* `displayRotation` — _optional_ `number` In degrees from North
* `passphraseAlwaysOnDevice` — _optional_ `boolean` Do not prompt for passphrase, enforce device entry
*   `safetyChecks` — _optional_ numeric `Enum_SafetyCheckLevel` value. Select the level deliberately; it changes device safety checks.

    <pre><code>0: Strict
    <strong>1: PromptAlways
    </strong>2: PromptTemporarily
    </code></pre>
* `experimentalFeatures` — _optional_ `boolean` Enable experimental message types

## Example

```typescript
HardwareSDK.deviceSettings(connectId, {
    label: 'My OneKey Wallet'
});
```

## Result

```typescript
{
    success: true,
    payload: {
        message: string
    }
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code?: string | number // error code
    }
}
```
