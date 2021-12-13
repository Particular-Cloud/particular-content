---
title: JavaScript Documentation
description: Our JavaScript npm packages is tailored towards a fast variety of different use cases. This article provides an overview over the @particular.cloud/i18n-js npm package and its usage.
---

# JavaScript (`i18n-js`) Documentation

Our JavaScript npm packages is tailored towards a vast variety of different use cases and it fully supports TypeScript, too!

This documentation provides an overview over the `@particular.cloud/i18n-js` npm package and its usage.

Find our full JavaScript/TypeScript API documentation [here](/documentation/developers/v1/javascript-api).

If you are using React, check out our [React documentation](/documentation/developers/v1/react).

---

## Installation

### npm

```bash
npm i @particular.cloud/i18n-js
# install the command-line interface (cli) to fetch translations during build time
npm i -D particular.cloud
```

### yarn

```bash
yarn add @particular.cloud/i18n-js
# install the command-line interface (cli) to fetch translations during build time
npm i -D particular.cloud
```

---

## CLI integration

Integrate our CLI tool to fetch translations during build time. This is optional but the most straight forward way to get translations loaded in your application. If you know what you are doing, you can also ditch the `particular.cloud` package and use `i18n-js` package directly. Either way, you will need a read-only token!

### Read-only token

Read-only token are used by your application to authenticate with Particular.Cloud. Additionally, our CLI tool uses this token to fetch texts from Particular.Cloud.

Navigate to the settings page of your project and create a read-only token. Find more information about how to create a token in the [developer documentation](/documentation/developers/v1).

**Note:** You can commit your **read-only** tokens to public repositories and to your client-side applications without fear.

To make the CLI tool work, add the following to your package.json in your project root folder:

```json
  "particular": {
    "token": "<read-only-token>",
    "defaultLanguage": "en-US"
  }
```

Navigate back to your browser window and copy the read-only token to your clipboard.

Awesome! Let's replace `<read-only-token>` with the token from our clipboard. And our CLI tool is ready to go! 🚀

---

### Load your texts during build time

Run `npx particular.cloud texts` to load your texts from Particular.Cloud into your `node_modules` folder.

Navigate to `node_modules/@particular.cloud/texts/dist/index.js` to enjoy a sneak peak of the loaded texts.

Let's automate this process by adding a postinstall command to the `package.json` file:

```json
  "scripts": {
    "postinstall": "particular.cloud texts"
  }
```

**Note:** The cli now runs as a postinstall script. If you deploy your code, the CLI should be executed automatically on `npm i`. In case your deployment process does not install devDependencies, make sure to install `particular.cloud` as a dependency. You can also use npx instead during your build process.

```json
  "scripts": {
    "postinstall": "npx particular.cloud texts"
  }
```

## Integrate i18n-js into your application

Now let's get started with our application! Import the `i18n` package and call the `init` function:

```javascript
const i18n = require('@particular.cloud/i18n-js');

i18n.init();
```

**Note:** `init` has to be called only once during your application's setup.

---

## Translations

Call the [t (translation)](/documentation/developers/v1/javascript/t) function and pass it your translation's key and the desired language.

```javascript
const string = i18n.t({ key: 'landingPageTitle', language: 'en' });
```

**Note:** When you create a new text on [Particular.Cloud](https://particular.cloud), you have to specify an unique key, which identifies the text across the different languages. In your application, the key is used to query for the text as well!

Wasn't that straight forward? Welcome to easy translation management! 🌟

---

## Configuration

`i18n-js` supports a wide variety of configuration options. You can adapt our i18n package to your needs.

In most cases, there are only a few options that should be interesting. Just pass an config object to the [init](/documentation/developers/v1/javascript-api#init) function:

```javascript
const i18n = require('@particular.cloud/i18n-js');

// use the Accept-Language header to determine the language
const userAcceptedLanguages = 'en-US,en;q=0.9,de-DE;q=0.8,de;q=0.7';
i18n.init({ defaultLanguage: 'en-US', acceptLanguage: userAcceptedLanguages });

const string = i18n.t({ key: 'landingPageTitle' });
```

**Note:** You don't even need to specify the language in the t (translation) function call when you make use of the defaultLanguage and acceptLanguage configuration options!

Find a description of all possible configuration options at the [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig) type defition.

## Fetch translations

Remember the read-only token that we used for our CLI tool? Let's integrate the token into our application. Pass the token to your `init` function:

```javascript
const userAcceptedLanguages = 'en-US,en;q=0.9,de-DE;q=0.8,de;q=0.7';
i18n.init({ defaultLanguage: 'en-US', acceptLanguage: userAcceptedLanguages, token: '<read-only-token>' });
```

Now you can call the [fetchT (fetchTranslation)](/documentation/developers/v1/javascript-api#fetcht) function to dynamically fetch translations from Particular.Cloud:

```javascript
// fetch translations during run time in your application!
const string = await i18n.fetchT({ key: 'landingPageTitle' });
```

We even cache your fetched texts in memory! Our i18n integration is on fire! 🔥🔥🔥

## Optional: VS Code extension

If you are using VS Code, get yourself the Particular.Cloud [VS Code extension](https://marketplace.visualstudio.com/items?itemName=particular-cloud.particular-cloud) to add some magic to your development workflow!

Read more about it in our [VS Code documentation](/documentation/developers/v1/vscode)!