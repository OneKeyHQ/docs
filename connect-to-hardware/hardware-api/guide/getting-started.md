# Getting Started

## Installation

Install library as npm module:

```shell
npm install @onekeyfe/connect
```

or

```shell
yarn add @onekeyfe/connect
```

### Initialization

ES6

```javascript
import OneKeyConnect from '@onekeyfe/connect';
```

CommonJS

```javascript
var OneKeyConnect = require('@onekeyfe/connect').default;
```

### API methods

* List of methods

### Handling events

* Events

### Running local version (develop/stable)

* clone repository: `git clone git@github.com:OneKeyHQ/connect.git`
* install node\_modules: `yarn`
* run localhost server: `yarn dev`

Initialize in project

```javascript
OneKeyConnect.init({
    connectSrc: 'https://localhost:8088/',
    lazyLoad: true, // this param will prevent iframe injection until OneKeyConnect.method will be called
    manifest: {
        email: 'hi@onekey.so',
        appUrl: 'https://onekey.so',
    }
})
```

### Running local version (custom branch)

In order to run a branch which isn't published to npm registry and this branch requires changes (mostly happened when new a method is added to OneKeyConnect interface)

* `git checkout custom-feature-branch`
* `yarn build:npm`

Install builded lib in your project:

#### Using `yarn link`

* `cd ./npm && yarn link`
* Inside your project: `yarn install @onekeyfe/connect`

#### Using local files

* Inside your project: `yarn install @onekeyfe/connect@file:/[local-path-to-repository]/npm`
