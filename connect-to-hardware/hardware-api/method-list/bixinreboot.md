# bixinReboot

import Playground from "@src/components/playground";

### Reboot device to bootloader

Reboot device to bootloader.

ES6

```javascript
const result = await OneKeyConnect.bixinReboot(params);
```

CommonJS

```javascript
OneKeyConnect.bixinReboot(params).then(function(result) {

});
```

#### Example

```javascript
OneKeyConnect.bixinReboot();
```

#### Result

```javascript
{
    success: true,
    payload: {
        message: 'device reboot started'
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
