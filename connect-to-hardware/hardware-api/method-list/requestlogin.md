# requestLogin

import Playground from "@src/components/playground";

### Request login

Challenge-response authentication via OneKey. To protect against replay attacks you should use a server-side generated and randomized `challengeHidden` for every attempt. You can also provide a visual challenge that will be shown on the device.

Service backend needs to check whether the signature matches the generated `challengeHidden`, provided `challengeVisual` and stored `publicKey` fields. If that is the case, the backend either creates an account (if the `publicKey` identity is seen for the first time) or signs in the user (if the `publicKey` identity is already a known user).

To understand the full mechanics, please consult the Challenge-Response chapter of [SLIP-0013: Authentication using deterministic hierarchy](https://github.com/satoshilabs/slips/blob/master/slip-0013.md).

ES6

```javascript
const result = await OneKeyConnect.requestLogin(params);
```

CommonJS

```javascript
OneKeyConnect.requestLogin(params).then(function(result) {

});
```

#### Params

**Optional common params**

Common parameter `useEmptyPassphrase` - is always set to `true` and it will be ignored by this method

**Login using server-side async challenge**

* `callback` — _required_ `function` which will be called from API to fetch `challengeHidden` and `challengeVisual` from server

**Login without async challenge**

* `challengeHidden` - _required_ `string` hexadecimal value
* `challengeVisual` - _required_ `string` text displayed on OneKey

#### Example

**Login using server-side async challenge**

```javascript
OneKeyConnect.requestLogin({
    callback: function() {
        // here should be a request to server to fetch "challengeHidden" and "challengeVisual"
        return {
            challengeHidden: '0123456789abcdef',
            challengeVisual: 'Login to',
        }
    }
})
```

**Login without async challenge**

```javascript
OneKeyConnect.requestLogin({
    challengeHidden: '0123456789abcdef',
    challengeVisual: 'Login to',
})
```

#### Result

```javascript
{
    success: true,
    payload: {
        address: string,
        publicKey: string,
        signature: string,
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

\<Playground initValue={ `OneKeyConnect.requestLogin({ challengeHidden: '0123456789abcdef', challengeVisual: 'Login to', })`} />
