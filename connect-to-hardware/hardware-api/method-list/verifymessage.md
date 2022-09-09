# verifyMessage

import Playground from "@src/components/playground";

### Wipe device

Reset device to factory defaults and remove all private data.

ES6

```javascript
const result = await OneKeyConnect.wipeDevice(params);
```

CommonJS

```javascript
OneKeyConnect.wipeDevice(params).then(function(result) {

});
```

#### Params

**Optional common params**

* Common parameter `useEmptyPassphrase` - is set to `true`
* Common parameter `allowSeedlessDevice` - is set to `true`

#### Example

```javascript
OneKeyConnect.wipeDevice();
```

#### Result

```javascript
{
    success: true,
    payload: {
        message: 'Device wiped'
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
