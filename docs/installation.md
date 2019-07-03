---
id: installation
title: Installation
sidebar_label: Installation
---

Note: unmock documentation uses yarn, but npm will also work. You can compare yarn and npm [here](https://yarnpkg.com/en/docs/migrating-from-npm#toc-cli-commands-comparison).

## CLI

### Global installation

#### Yarn

```bash
yarn global add unmock
```

#### npm

```bash
npm install -g unmock
```

#### Homebrew

```
brew tap unmock/unmock
brew install unmock
```

### Install in project

#### Yarn

```text
yarn add unmock -D
```

Execute with

```text
yarn unmock
```

#### npm

```text
npm install unmock --save-dev
```

To execute, install [npx](https://www.npmjs.com/package/npx)

```text
npm install -g npx
npx unmock
```

or

```text
./node_modules/bin/unmock
```

## Node.js

### Jest

Install jest-unmock:

```text
yarn add -D jest-unmock
```

Add jest-unmock as preset in`jest.config.json`:

```text
// jest.config.json
{
  "preset": ["jest-unmock"]
}
```

or in `package.json`:

```text
// package.json
"jest": {
  "preset": ["jest-unmock"]
}
```

If you're already using another preset like `ts-jest`, add jest-unmock in `setupFilesAfterEnv`:

```text
// package.json
"jest": {
  "setupFilesAfterEnv": ["jest-unmock/jest-setup.js"],
}
```

Now whenever Jest runs, unmock will capture all outgoing HTTP requests and serve local mocks from `__unmocks__` folder.

### Ava

TODO.
