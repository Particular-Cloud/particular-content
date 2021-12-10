---
title: CLI Documentation
description: Particluar.Cloud offers an extensible REST API to read and write from and to your Particluar.Cloud project.
---

# Command-Line Interface Documentation

Our command-line interface (CLI) can be used standalone or together with our `i18n-js` or `i18n-react` packages. It is the easiest way to integrate Particular.Cloud with your existing i18n packages such as [i18next](https://www.i18next.com/) or [react-i18next](https://react.i18next.com/).

Check out our [`i18n-js` JavaScript documentation](/documentation/developers/v1/javascript) or our [`i18n-react` React documentation](/documentation/developers/v1/react) for more information on how to integrate the CLI together with our packages.

For more information on how to use the CLI standalone, follow along! 🤓

## Installation

### npm

```bash
# this is where you can access your localized texts
npm i @particular.cloud/texts
# this is our CLI package
npm i -D particular.cloud
```

### yarn

```bash
# this is where you can access your localized texts
npm i @particular.cloud/texts
# this is our CLI package
npm i -D particular.cloud
```

---

## Authentification

Any form of communication between an application and Particluar.Cloud requires you to create a project token for your Particular.Cloud project.

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

## Load your texts during build time

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

## Access your project texts in your application

```javascript
import { keys, texts, languages } from '@particular/texts';
```

Now you can access your texts in your application! 🔥

```javascript
console.log(keys);

// {
//   "title": "title",
//   "title2": "title2",
//   "descriptionSmall": "descriptionSmall",
//   "loginButton": "loginButton",
//   "registerButton": "registerButton",
// }
```

Use your texts with your favorite i18n package:

```javascript
console.log(texts);

// {
//   "en-US": {
//     "title": "Homepage",
//     "title2": "Welcome back!",
//     "descriptionSmall": "Get started or create an account to get started.",
//     "loginButton": "Signin",
//     "registerButton": "Signup"
//   },
//   "de-DE": {
//     "title": "Homepage",
//     "title2": "Willkommen zurück!",
//     "descriptionSmall": "Starte durch oder erstelle einen neuen Account.",
//     "loginButton": "Einloggen",
//     "registerButton": "Registrieren"
//   },
//   "en-GB": {
//     "title": "Homepage",
//     "title2": "Welcome back!",
//     "descriptionSmall": "Get started or create an account to get started.",
//     "loginButton": "Login",
//     "registerButton": "Register"
//   }
// }
```

You can use the `languages` array to display a list of all supported languages to your users:

```javascript
console.log(languages);

// [
//   {
//     "isDefault": true,
//     "isActive": true,
//     "language": {
//       "locale": "en-US",
//       "langCode": "en",
//       "langName": "English",
//       "langNameLocal": "English",
//       "countryCode": "US",
//       "countryName": "United States of America",
//       "countryNameLocal": "United States of America",
//       "flag": "🇺🇸"
//     }
//   },
//   {
//     "isDefault": true,
//     "isActive": true,
//     "language": {
//       "locale": "de-DE",
//       "langCode": "de",
//       "langName": "German",
//       "langNameLocal": "Deutsch",
//       "countryCode": "DE",
//       "countryName": "Germany",
//       "countryNameLocal": "Deutschland",
//       "flag": "🇩🇪"
//     }
//   },
//   {
//     "isDefault": false,
//     "isActive": false,
//     "language": {
//       "locale": "en-GB",
//       "langCode": "en",
//       "langName": "English",
//       "langNameLocal": "English",
//       "countryCode": "GB",
//       "countryName": "United Kingdom",
//       "countryNameLocal": "United Kingdom",
//       "flag": "🇬🇧"
//     }
//   }
// ]
```

## Debugging

Run `npx particular.cloud texts --loglevel debug` to run the CLI in debug mode.

For a full list of all CLI options, run `npx particular.cloud`.