# Blockchain Utility Tools

## Overview

- Some modules need to be Browserify.

## Browserify

```sh
yarn
browserify bip39.js -s bip39 -o ../js/bip39.js
browserify borsh.js -s borsh -o ../js/borsh.js
browserify bs58.js -s bs58 -o ../js/bs58.js
browserify buffer.js -s Buffer -o ../js/buffer.js
browserify ed25519-hd-key.js -s ed25519HdKey -o ../js/ed25519-hd-key.js
browserify hdkey.js -s HDKey -o ../js/hdkey.js
```

## Known Issues

- We cannot browserify the borsh module because of [Logical OR assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment). And here is the way to workaround:

```javascript
// file: browserify/node_modules/@solana/web3.js/lib/index.browser.cjs.js

// line 632
// change from
keyMeta.isSigner ||= accountMeta.isSigner;
// to
keyMeta.isSigner = keyMeta.isSigner || accountMeta.isSigner;

// line 633
// change from
keyMeta.isWritable ||= accountMeta.isWritable;
// to
keyMeta.isWritable = keyMeta.isWritable || accountMeta.isWritable;

// line 1809
// change from
(errors.missing ||= []).push(publicKey);
// to
errors.missing = errors.missing || [];
errors.missing.push(publicKey);

// line 1813
// change from
(errors.invalid ||= []).push(publicKey);
// to
errors.invalid = errors.invalid || [];
errors.invalid.push(publicKey);

// line 7547
// change from
const stateChangeCallbacks = this._subscriptionStateChangeCallbacksByHash[hash] ||= new Set();
// to
this._subscriptionStateChangeCallbacksByHash[hash] = this._subscriptionStateChangeCallbacksByHash[hash] || new Set();
const stateChangeCallbacks = this._subscriptionStateChangeCallbacksByHash[hash];
```
