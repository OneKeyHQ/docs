# Debug Logging

You can print out internal communication messages between provider and OneKey wallet. This feature may be useful for your application debugging and development.

Logging messages have been grouped by **jsBridge**, **providerBase** and **extInjected.** You can toggle them on or off like this, take effect after page reload:&#x20;

```javascript
// turn on all groups logging
localStorage.setItem('$$ONEKEY_DEBUG_LOGGER', '*');

// turn off all groups logging
localStorage.setItem('$$ONEKEY_DEBUG_LOGGER', '');

// turn on `jsBridge` group only
localStorage.setItem('$$ONEKEY_DEBUG_LOGGER', 'jsBridge');

// turn on both `jsBridge` and `providerBase` group
localStorage.setItem('$$ONEKEY_DEBUG_LOGGER', 'jsBridge,providerBase');

```
