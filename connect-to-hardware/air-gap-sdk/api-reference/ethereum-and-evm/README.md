# Ethereum & EVM

To obtain an ETH account, you can use the basic API [`CryptoHDkey`](../basic-api/cryptohdkey.md) to acquire the account.



When integrating ETH, you can request a signature from the hardware using `EthSignRequest`. Currently supported are:

* EIP1559 Transaction
* Legacy Transaction
* EIP-712 TypedData
* Sign Message

For details on how to generate the corresponding QR codes, you can refer to the relevant documentation.

{% content-ref url="ethsignrequest.md" %}
[ethsignrequest.md](ethsignrequest.md)
{% endcontent-ref %}





Regardless of the type of signature, the result is `EthSignature`.

{% content-ref url="ethsignature.md" %}
[ethsignature.md](ethsignature.md)
{% endcontent-ref %}



