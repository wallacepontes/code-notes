# Obfuscator

## Table of Contends

- [Obfuscator](#obfuscator)
  - [Table of Contends](#table-of-contends)
  - [1) Quick overview — what is obfuscation?](#1-quick-overview--what-is-obfuscation)
  - [2) Common tools](#2-common-tools)
  - [3) Install](#3-install)
  - [4) Basic CLI usage](#4-basic-cli-usage)
  - [5) Programmatic usage (Node)](#5-programmatic-usage-node)
  - [6) Key `javascript-obfuscator` options (practical)](#6-key-javascript-obfuscator-options-practical)
  - [7) Example: small before/after](#7-example-small-beforeafter)
  - [8) Integrating with build tools](#8-integrating-with-build-tools)
    - [Webpack](#webpack)
    - [Rollup](#rollup)
    - [esbuild](#esbuild)
  - [9) Source maps and debugging](#9-source-maps-and-debugging)
  - [10) CI/CD and release considerations](#10-cicd-and-release-considerations)
  - [11) Performance \& size](#11-performance--size)
  - [12) What *not* to obfuscate](#12-what-not-to-obfuscate)
  - [13) Legal \& ethical](#13-legal--ethical)
  - [14) Troubleshooting](#14-troubleshooting)
  - [15) Minimal recommended config (practical)](#15-minimal-recommended-config-practical)
  - [16) Sample full flow (example)](#16-sample-full-flow-example)
  - [17) Alternatives \& complements](#17-alternatives--complements)
  - [18) Quick checklist before shipping](#18-quick-checklist-before-shipping)
  - [References](#references)

Nice — let’s get you started with **code obfuscation in Node.js**. I’ll walk you through what obfuscation does (and doesn’t), show the most common tools, give working examples (CLI + programmatic + build-tool integration), and finish with best practices, pitfalls, and troubleshooting tips.

## 1) Quick overview — what is obfuscation?

Obfuscation transforms readable source code into a functionally-equivalent but much harder-to-read form.
**Important:** obfuscation *is not* a security measure for secrets or authentication—it only raises the effort required to understand your JS. Don’t store private keys or secrets in obfuscated client-side code.

## 2) Common tools

* **javascript-obfuscator** — popular, feature-rich JS obfuscator (works for browser and Node bundles).
* **Terser / UglifyJS** — minifiers (rename symbols, remove whitespace) — lighter weight than full obfuscation.
* **esbuild/rollup/webpack plugins** — integrate obfuscation into your build pipeline.

For most Node.js apps that produce JS bundles (e.g., an electron app or a shipped CLI built as a bundle), `javascript-obfuscator` is the usual choice.

## 3) Install

If you want the library and CLI:

```bash
npm install --save-dev javascript-obfuscator
# or
yarn add -D javascript-obfuscator
```

## 4) Basic CLI usage

Obfuscate a file:

```bash
npx javascript-obfuscator input.js --output obf.js
```

Simple options in CLI:

```bash
npx javascript-obfuscator input.js --output obf.js \
  --compact true \
  --control-flow-flattening true \
  --string-array true \
  --string-array-encoding base64 \
  --rotate-string-array true
```

## 5) Programmatic usage (Node)

You can obfuscate programmatically (e.g., inside a build script):

```js
// scripts/obfuscate.js
const fs = require('fs');
const JavaScriptObfuscator = require('javascript-obfuscator');

const input = fs.readFileSync('dist/bundle.js', 'utf8');

const obfuscated = JavaScriptObfuscator.obfuscate(
  input,
  {
    compact: true,
    controlFlowFlattening: true,
    controlFlowFlatteningThreshold: 0.75,
    stringArray: true,
    stringArrayThreshold: 0.75,
    stringArrayEncoding: ['base64'],
    rotateStringArray: true,
    disableConsoleOutput: true,
    // ...more options below
  }
);

fs.writeFileSync('dist/bundle.obf.js', obfuscated.getObfuscatedCode());
console.log('Obfuscation complete');
```

Add to package.json:

```json
"scripts": {
  "build": "webpack --mode=production",
  "obfuscate": "node scripts/obfuscate.js",
  "release": "npm run build && npm run obfuscate"
}
```

## 6) Key `javascript-obfuscator` options (practical)

* `compact` — remove whitespace/newlines.
* `controlFlowFlattening` — tougher: converts code control flow into a flattened, less-intuitive form (large performance cost).
* `controlFlowFlatteningThreshold` — probability (0..1) for applying it to AST nodes.
* `deadCodeInjection` — inject fake unreachable code (increases size).
* `stringArray` — extract strings into array to be referenced by index.
* `stringArrayEncoding` — e.g., `['base64']` or `['rc4']` (RC4 adds cost + obfuscation).
* `rotateStringArray` — slight extra obfuscation.
* `transformObjectKeys` — obfuscates object keys.
* `disableConsoleOutput` — remove console.* calls.
* `sourceMap` — generate source maps (useful in dev, remove in production if you want to be harder to reverse-engineer).

**Tradeoff**: stronger options = harder to reverse but larger files and slower runtime. Balance is needed.

## 7) Example: small before/after

Input `hello.js`:

```js
function greet(name) {
  console.log('Hello, ' + name + '!');
}
greet('Wallace');
```

After obfuscation (example fragment):

```js
var _0x3b2f=['Hello, ','log'];(function(_0x5ff5f6,_0x3b2f1a){...})(_0x3b2f,0x1f4);function _0x1a2b(_0x5076){console[_0x2aa(0x1)](_0x2aa(0x0)+_0x5076+'!');} _0x1a2b('Wallace');
```

(Real output depends on options, this is representative.)

## 8) Integrating with build tools

### Webpack

Use a post-build script, or a plugin that runs javascript-obfuscator after emit.
Example using `webpack` with `afterEmit` hook in a simple plugin (or run `obfuscate` script after `webpack` finishes):

```js
// package.json
"scripts": {
  "build": "webpack --mode=production",
  "postbuild": "node scripts/obfuscate.js"
}
```

There are also community plugins like `webpack-obfuscator` — if used, pin their versions and test thoroughly.

### Rollup

Run obfuscation as a plugin or as a post-build step. `rollup-plugin-obfuscator` exists in the ecosystem; if you use it, prefer executing obfuscation after code is bundled.

### esbuild

esbuild is fast but doesn't have first-class obfuscation. Use esbuild to bundle, then run `javascript-obfuscator` on the resulting bundle.

## 9) Source maps and debugging

* **Dev**: keep source maps enabled (`sourceMap: true`) so your stack traces map to original files.
* **Production**: **do not** ship source maps if you want code harder to reverse. If you must keep source maps for error reporting, store them privately (e.g., upload to an error service) — *do not* serve them to end users.

## 10) CI/CD and release considerations

* Add obfuscation as a step in your CI release pipeline (after building and tests).
* Keep a non-obfuscated copy in your protected artifacts for debugging.
* Use deterministic builds where possible (seed any randomness in obfuscation) to debug regressions. Note: `javascript-obfuscator` has a `seed` option to make output repeatable.

## 11) Performance & size

* Obfuscation (especially control flow flattening and dead code) increases file size and CPU cost. Run performance tests with your obfuscated build (especially for CPU-sensitive apps like Electron or serverless cold starts).
* If runtime cost is unacceptable, prefer minification (Terser) and selective obfuscation (only for parts you ship externally).

## 12) What *not* to obfuscate

* Secrets (API keys, secrets) — never. Use proper secret management.
* Licenses or legal text that must remain readable (where applicable).
* Server-side code you control — if code runs only on your servers, obfuscation is unnecessary (and may hinder debugging).

## 13) Legal & ethical

* Obfuscation may affect open-source license compliance if you relicense or ship code with copyleft requirements. Ensure you comply with all licenses of code you redistribute.
* If your app must be auditable (financial, safety-critical), obfuscation may be inappropriate.

## 14) Troubleshooting

* **App breaks after obfuscation**: disable aggressive features (controlFlowFlattening, transformObjectKeys) and re-run. Enable `debugProtection` only for client builds where supported.
* **Huge bundle**: turn off `deadCodeInjection` and high thresholds for string array.
* **Source map mismatch**: ensure obfuscator generates maps from the bundled file and they’re aligned. You might need to generate sourcemaps in the bundler and pass them through.

## 15) Minimal recommended config (practical)

If you want a decent level of obfuscation without huge performance hit:

```js
{
  compact: true,
  controlFlowFlattening: false, // set true only if you accept perf cost
  stringArray: true,
  stringArrayThreshold: 0.8,
  stringArrayEncoding: ['base64'],
  rotateStringArray: true,
  disableConsoleOutput: true
}
```

## 16) Sample full flow (example)

1. Write code.
2. Run tests.
3. Bundle via webpack/esbuild/rollup → `dist/bundle.js`.
4. Run obfuscation script on `dist/bundle.js` to produce `dist/bundle.obf.js`.
5. Deploy `bundle.obf.js` (don’t include source maps publicly).

## 17) Alternatives & complements

* **Minification (Terser)** for size and modest obfuscation.
* **Native addons** (C/C++/Rust) for extremely sensitive logic (harder to reverse but adds complexity).
* **Server-side enforcement** — keep sensitive logic on trusted servers whenever possible.

## 18) Quick checklist before shipping

* [ ] All secrets removed from shipped code.
* [ ] Build and obfuscation step included in CI.
* [ ] Performance tested with obfuscated build.
* [ ] No public source maps (unless intentionally).
* [ ] License compliance verified.

---

## References
1. https://github.com/javascript-obfuscator/javascript-obfuscator
2. 