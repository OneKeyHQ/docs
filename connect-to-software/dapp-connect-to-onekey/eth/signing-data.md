# Signing Data

Since OneKey makes cryptographic keys available to each user, websites can use these signatures for a variety of uses. Here are a few guides related to specific use cases:

* [Authenticating websites](https://medium.com/hackernoon/writing-for-blockchain-wallet-signature-request-messages-6ede721160d5)

### Signing Data with OneKey <a href="#signing-data-with-onekey" id="signing-data-with-onekey"></a>

If you’d like to jump to some working signature examples, [you can visit this repository](https://github.com/danfinlay/js-eth-personal-sign-examples).

If you’d like to read our JavaScript implementations of these methods, they are all available in the npm package [eth-sig-util](https://github.com/onekeyhq/eth-sig-util).

Note that OneKey supports signing transactions with Trezor and Ledger hardware wallets. These hardware wallets currently only support signing data using the `personal_sign` method. If you have trouble logging in to a website or dapp when using a Ledger or Trezor, the site may be requesting you sign data via an unsupported method, in which case we recommend using your standard OneKey account.

### A Brief History <a href="#a-brief-history" id="a-brief-history"></a>

There are currently six signing methods in OneKey, and you might wonder the history of these methods. Studying the history of these methods has some lessons in it for the emergent lessons of decentralized standards emergence. Our current five methods are:

* `eth_sign` (**Deprecated,** unsafety)
* `personal_sign`
* `signTypedData` (currently identical to `signTypedData_v1`)
* `signTypedData_v1`
* `signTypedData_v3`
* `signTypedData_v4`

There are likely to be many more over time. When OneKey first started, the Provider API wasn’t designed to be exposed to untrusted websites, and so some considerations weren’t taken as seriously as they were later.

In particular, the method `eth_sign` is an open-ended signing method that allows signing an arbitrary hash, which means it can be used to sign transactions, or any other data, making it a dangerous phishing risk.

For this reason, we make this method show the most frightening possible message to the user, and generally discourage using this method in production. However, some applications (usually admin panels internal to teams) use this method for the sake of its ease of use, and so we have continued to support it for the sake of not breaking the workflows of active projects.

Eventually, the `personal_sign` [spec](https://github.com/ethereum/go-ethereum/pull/2940) was proposed, which added a prefix to the data so it could not impersonate transactions. We also made this method able to display human readable text when UTF-8 encoded, making it a popular choice for site logins.

However, the text-prefix made those signatures expensive to verify on-chain, and so with the help of the [0xProtocol](https://0x.org/) team and [SpankChain](https://spankchain.com/), the [EIP-712](https://eips.ethereum.org/EIPS/eip-712) spec was written.

The strange part of EIP-712, and this decentralized standards ecosystem, is that the proposal changed several times while retaining the same EIP. This means what we initially implemented as `signTypedData` was the earliest proposed version, while other groups implemented later versions under the same method name.

To avoid compatibility issues between clients, we recommend using the hard-versioned method names `signTypedData_v1` and `signTypedData_v3`. The missing `v2` represents an intermediary design that was implemented by the Cipher browser, so that we have room to implement it if there is ever enough developer demand for it.

In the future, it may help to have method names include a hash of their exact proposal, since in a decentralized ecosystem, there is no absolute source of truth of what a given name should map to. Instead, we are forced to invent new patterns of collaboration, where we can drive forward and innovate, while simultaneously avoiding creating a brittle ecosystem by changing our meanings out from under the words.

I hope this has been a useful introduction to the history of our signing methods!

### Sign Typed Data v1 <a href="#sign-typed-data-v1" id="sign-typed-data-v1"></a>

This early version of the spec lacked some later security improvements, and should generally be neglected in favor of [signTypedData\_v3](signing-data.md).

The `signTypedData` family has a few major design considerations:

* Cheap to verify on chain
* Still somewhat human readable
* Hard to phish signatures

If on-chain verifiability cost is a high priority for you, you might want to consider it.

### Sign Typed Data v3 <a href="#sign-typed-data-v3" id="sign-typed-data-v3"></a>

The method `signTypedData_v3` currently represents the latest version of the [EIP-712 spec](https://eips.ethereum.org/EIPS/eip-712), making it the most secure method for signing cheap-to-verify data on-chain that we have yet.

This does not mean it is perfect, and we do already have a `v4` in prototype stage (which supports recursive structs and arrays), but we do intend to protect this namespace and keep it compatible going forwards.

Hopefully soon we will also have good examples for parsing method input into structs for verification on-chain (great contribution opportunity!)

### Sign Typed Data v4 <a href="#sign-typed-data-v4" id="sign-typed-data-v4"></a>

The method `signTypedData_v4` currently represents the latest version of the [EIP-712 spec](https://eips.ethereum.org/EIPS/eip-712), with added support for arrays and with a breaking fix for the way structs are encoded.

This does not mean it is perfect, and does not mean we will not eventually have a `v5`, but we do intend to protect this namespace and keep it compatible going forwards.

Hopefully soon we will also have good examples for parsing method input into structs for verification on-chain (great contribution opportunity!)

#### Sign Typed Data Message Parameters <a href="#sign-typed-data-message-parameters" id="sign-typed-data-message-parameters"></a>

`domain`: The Domain or domain signature is important because it:

* Will only be accepted for a specific website/contract.
* Makes sure signatures are valid only where they are intended to be valid.
* Allows you have a unique contract that verifies the address.
* This is a bunch of information that restricts where the signature is valid.
* This is the domain of validity. Could be a contract, a url, ect.
* What needs to be put in here specifically what the DApp tells you.
* Makes sure your signature(s) don't collide with other signatures.

`chainId`: The chainId tell you what chain you're on and this is important because:

* It makes sure signatures signed on Rinkeby are not valid on another chain, such as the Ethereum Main Net.

`name`: This is primarily for UX(User Experience) purposes.

* For example, as a user, you're using an Ether Mail app and a dialog comes up for cryptokitties exchange, this would arouse suspicion due to what the name is on the signature.

`verifyingContract`: This is an extra layer of assurance. Even if two developers end up creating an app with the same name, they will never have the same contract address.(You can add another field `salt` but it's complete overkill and unnecessary)

* If you are unsure of the name this will show the contract responsible for message verification.
* This field will also take a url.

`version`: This tell you the current version of the domain object.

`message`: Completely open to what you would like the structure of it to be. Every field is optional.

Below is an example of signing typed data with OneKey. Reference [here](https://github.com/danfinlay/js-eth-personal-sign-examples)

#### Example <a href="#example" id="example"></a>

{% embed url="https://codesandbox.io/embed/ethereum-provider-signdata-demo-67nn3q?autoresize=1&fontsize=14&hidenavigation=1&theme=dark" fullWidth="true" %}

