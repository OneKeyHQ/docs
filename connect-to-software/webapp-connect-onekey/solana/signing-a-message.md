# Signing a Message

When a web application is connected to OneKey, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within OneKey's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

In order to send a message for the user to sign, a web application must:

1. Provide the message bytes as a `Uint8Array`; use `TextEncoder` for UTF-8 text.
2. Request that the encoded message is signed via the user's OneKey wallet.

{% tabs %}
{% tab title="signMessage" %}
```javascript
const provider = getProvider();
if (!provider) throw new Error('OneKey Solana provider is unavailable.');
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request" %}
```javascript
const provider = getProvider();
if (!provider) throw new Error('OneKey Solana provider is unavailable.');
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params: {
         message: bs58.encode(encodedMessage),
         display: "utf8",
    },
});
```
{% endtab %}
{% endtabs %}

The `request` example requires `import bs58 from "bs58"`. Both forms resolve to an object with a `Uint8Array` `signature` and a `PublicKey` `publicKey`. The display option controls presentation; it does not change the bytes being signed. For authentication, verify the signature against the exact message and use a single-use challenge.
