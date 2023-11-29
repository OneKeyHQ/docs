# lnurl

Request to execute a [LNURL](https://github.com/lnurl/luds) request. The application needs to pass a [LNURL](https://github.com/lnurl/luds/blob/luds/01.md) string which should be provided for example by the application's backend. The lnurl function can also accept a [LUD-16](https://github.com/lnurl/luds/blob/luds/16.md) static identifier (e.g. username@gmail.com) instead of a LNURL string.

The method returns a promise which resolves once the LNURL flow is completed. It returns the last response from the LNURL server. For LNURL-pay requests it also contains payment information (preimage, payment hash) and for LNURL-auth requests it contains auth information (message, signature)&#x20;

### Method

```typescript
async function lnurl(lnurl: string): LNURLResponse;
```

### Response

```typescript
type LNURLResponse =
  | {
      status: "OK";
      data?: unknown
    }
  | { status: "ERROR"; reason: string };
```



### Example

```typescript
const lnurl = "xxxx";
await window.$onekey?.webln.enable();
await window.$onekey?.webln.lnurl(lnurl);
```
