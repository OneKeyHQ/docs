# RPC API Calling

OneKey uses the `provider.request({ method, params })` method to wrap an [NEAR RPC API](https://docs.near.org/docs/api/rpc).

You can call any RCP API by this method, for examples:&#x20;

[https://docs.near.org/docs/api/rpc/network#node-status](https://docs.near.org/docs/api/rpc/network#node-status)

```javascript
const res = await provider.request({
  'method': 'status',
});
console.log('RPC Call: status', res);
```

[https://docs.near.org/docs/api/rpc/gas#gas-price](https://docs.near.org/docs/api/rpc/gas#gas-price)

```javascript
const res = await provider.request({
  'method': 'gas_price',
  'params': [null],
});
console.log('RPC Call: gas_price', res);
```

[https://docs.near.org/docs/api/rpc/access-keys#view-access-key-list](https://docs.near.org/docs/api/rpc/access-keys#view-access-key-list)

```javascript
const res = await provider.request({
  'method': 'query',
  'params': {
    'request_type': 'view_access_key_list',
    'account_id':
      'c3be856133196da252d0f1083614cdc87a85c8aa8abeaf87daff1520355eec53',
    'finality': 'optimistic',
  },
});
console.log('RPC Call: view_access_key_list', res);
```
