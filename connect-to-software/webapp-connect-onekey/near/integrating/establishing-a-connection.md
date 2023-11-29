# Establishing a Connection

In order to start interacting with OneKey you must first establish a connection. This connection request will prompt the user for permission to share their account id and public key, and indicate that they are willing to interact further.

Once permission is established the first time, the web application's domain will be whitelisted for future connection requests.&#x20;

Similarly, it is possible to terminate the connection both on the application and the user side.

## Connecting (SignIn)

The **recommended** and **easiest** way to connect to OneKey is by calling `provider.requestSignIn()` . However, the provider also exposes a `request` JSON RPC interface.

{% tabs %}
{% tab title="requestSignIn()" %}
```javascript
try {
  const res = await provider.requestSignIn();
  console.log('requestSignIn', res.accounts);
} catch(error){
  // { code: 4001, message: 'User rejected the request.' }
}
```
{% endtab %}

{% tab title="request()" %}
```javascript
try {
  const res = await provider.request({
    method: 'near_requestAccounts',
  });
  console.log('near_requestAccounts', res.accounts);
} catch(error){
  // { code: 4001, message: 'User rejected the request.' }
}
```
{% endtab %}
{% endtabs %}

## Disconnecting (SignOut)

Disconnecting is similar to connecting, however, it is possible for the disconnection to originate from the wallet.

{% tabs %}
{% tab title="signOut()" %}
```javascript
provider.signOut();
```
{% endtab %}

{% tab title="request()" %}
```javascript
provider.request({ method: 'near_signOut' });
```
{% endtab %}
{% endtabs %}
