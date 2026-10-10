# Cancel Request

## Cancel command

Cancels the execution of the currently active method and fires the `ui-close_window` event.

```typescript
HardwareSDK.cancel(connectId);
```

### Params

* `connectId` — optional `string`. Cancels a request for the specified connection. Consult the selected SDK entry point before omitting it.

### Examples

```typescript
HardwareSDK.cancel(connectId);
```

Result

```typescript
undefined
```
