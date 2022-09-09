# resetDevice

import Playground from '@src/components/playground';

### Reset device

Perform device setup and generate new seed.

ES6

```javascript
const result = await OneKeyConnect.resetDevice(params);
```

CommonJS

```javascript
OneKeyConnect.resetDevice(params).then(function (result) {});
```

#### Params

**Optional common params**

* `strength` — _optional_ `number` Accepted values are \[ 128 | 192 | 256 ]. Default is set to `256`
* `label` — _optional_ `string`
* `u2fCounter` — _optional_ `number`. Default value is set to current time stamp in seconds.
* `pinProtection` — _optional_ `boolean`
* `passphraseProtection` — _optional_ `boolean`
* `skipBackup` — _optional_ `boolean`
* `noBackup` — _optional_ `boolean` create a seedless device

#### Example

```javascript
OneKeyConnect.resetDevice({
    label: 'My fancy OneKey',
});
```

#### Result

```javascript
{
    success: true,
    payload: {
        message: 'Device successfully initialized'
    }
}
```

Error

```javascript
{
    success: false,
    payload: {
        error: string // error message
    }
}
```

\<Playground initValue={ `OneKeyConnect.resetDevice({ label: 'My fancy OneKey', });`} />
