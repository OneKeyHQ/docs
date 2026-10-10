# Aptos Wallet Adapter

## Choose a UI Design

{% tabs %}
{% tab title="Ant Design" %}
### Documentation

{% embed url="https://github.com/aptos-labs/aptos-wallet-adapter/tree/main/packages/wallet-adapter-ant-design" %}

```
npm install @aptos-labs/wallet-adapter-ant-design
```
{% endtab %}

{% tab title="MUI" %}
### Documentation

{% embed url="https://github.com/aptos-labs/aptos-wallet-adapter/tree/main/packages/wallet-adapter-mui-design" %}

```
npm install @aptos-labs/wallet-adapter-mui-design
```
{% endtab %}
{% endtabs %}

## Install

{% tabs %}
{% tab title="npm" %}
```
npm install @aptos-labs/wallet-adapter-react @onekeyfe/aptos-wallet-adapter
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add @aptos-labs/wallet-adapter-react @onekeyfe/aptos-wallet-adapter
```
{% endtab %}

{% tab title="pnpm" %}
```
pnpm install @aptos-labs/wallet-adapter-react @onekeyfe/aptos-wallet-adapter
```
{% endtab %}
{% endtabs %}

## Quickstart and Demo

{% embed url="https://codesandbox.io/embed/cz8rcy?module=/src/App.tsx&view=Editor+++Preview" fullWidth="true" %}

The adapter APIs depend on the installed major version. Check the [official Aptos Wallet Adapter documentation](https://github.com/aptos-labs/aptos-wallet-adapter) and the OneKey adapter package compatibility before combining packages. Install the core React adapter before the optional UI package.
