# Accessing Accounts

## Accessing Accounts

User accounts are used in a variety of contexts in Ethereum, including as identifiers and for signing transactions. In order to request a signature from the user or have the user approve a transaction, one must be able to access the user's accounts. The `wallet methods` below involve a signature or transaction approval and all require the sending account as a function parameter.

* `eth_sendTransaction`
* `eth_sign` (insecure and unadvised to use)
* `eth_personalSign`
* `eth_signTypedData`



**Connect Account Example:**

{% embed url="https://codesandbox.io/embed/ethereum-provider-demo-rwgl5l?fontsize=14&hidenavigation=1&theme=dark" fullWidth="true" %}
