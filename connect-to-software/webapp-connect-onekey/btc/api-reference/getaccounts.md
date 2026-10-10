# getAccounts

Read the addresses currently exposed to this site. To request access, use [requestAccounts](requestaccounts.md).

## Method

```typescript
declare function getAccounts(): Promise<string[]>;
```

### Example

```typescript
const provider = (window.$onekey && window.$onekey.btc) || window.unisat;

const accounts = await provider.getAccounts()
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/qBgMXRM" %}
