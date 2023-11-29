# Signing a Message

When a web application is connected to OneKey, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within OneKey's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

In order to send a message for the user to sign, a web application must:&#x20;

1. Provide a **hex** or **UTF-8** encoded string as a Uint8Array.
2. Request that the encoded message is signed via the user's OneKey wallet.

{% tabs %}
{% tab title="signMessage" %}
```javascript
const provider = getProvider();
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request" %}
```javascript
const provider = getProvider();
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params: {
         message: encodedMessage,
         display: "hex",
    },
});
```
{% endtab %}
{% endtabs %}
