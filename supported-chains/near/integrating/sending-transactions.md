# Sending Transactions

Once the web application is connected to OneKey, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the web application must:

* Create an unsigned transaction or transactions.
* Have it be signed and submitted to the network by the user's OneKey wallet.

For more information about the nature of transactions on Near, it is recommended to review the `near-api-js` [docs](https://docs.near.org/docs/develop/front-end/near-api-js) as well as the official [Near docs](https://docs.near.org/).

{% content-ref url="sending-transactions/create-transaction.md" %}
[create-transaction.md](sending-transactions/create-transaction.md)
{% endcontent-ref %}

{% content-ref url="sending-transactions/sign-and-send-transaction.md" %}
[sign-and-send-transaction.md](sending-transactions/sign-and-send-transaction.md)
{% endcontent-ref %}

Sometimes you may need signing transactions only but not sending by OneKey, and then sending with your application self.

{% content-ref url="sending-transactions/signing-transaction.md" %}
[signing-transaction.md](sending-transactions/signing-transaction.md)
{% endcontent-ref %}
