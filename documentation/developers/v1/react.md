---
title: React Documentation
description: Our React npm package supports static site generation (SSG), server-side rendering (SSR), and good-ol' fashioned client-side applications. This article provides an overview over the @particular.cloud/i18n-react npm package and its usage.
---

# React (i18n-react) Documentation

Our React npm package supports static site generation (SSG), server-side rendering (SSR), and good-ol' fashioned client-side applications.

This documentation provides an overview over the `@particular.cloud/i18n-react` npm package and its usage. Find our full React API documentation [here](/documentation/developers/v1/react-api).

If you are developing non-React JavaScript applications, check out our general [JavaScript documentation](/documentation/developers/v1/javascript).

---

## Installation

### npm

```bash
# install the React i18n sdk
npm i @particular.cloud/i18n-react
# install the command-line interface (cli) to fetch translations during build time
npm i -D particular.cloud
```

### yarn

```bash
# install the React i18n sdk
yarn add @particular.cloud/i18n-react
# install the command-line interface (cli) to fetch translations during build time
yarn add -D particular.cloud
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

---

## Integrate i18n-react into your React application

Open your `src/index.jsx` (or wherever you render your Application) in your code editor and import `I18nProvider` from `@particular.cloud/i18n-react`:

```jsx
import { I18nProvider } from '@particular/i18n-react';
```

The `I18nProvider` context provider is used to configure the i18n-react library. For that, let's wrap our App component with the I18nProvider:

```jsx
<I18nProvider config={{ defaultLanguage: 'en-US' }}>
  <App />
</I18nProvider>
```

You can pass I18nProvider an object with configuration options. Learn about the extensive configuration options on the Particular.Cloud [i18n-js developer documentation](/documentation/developers/v1/javascript-api#init).

For now, we just specify the default language for our users.

---

## Translations

Now you can use the useText hook or the Text component to display your localized text.

### useText hook

```jsx
import { useText } from '@particular.cloud/i18n-react';

return <h1>{useText({ key: 'landingPageTitle', language: 'en-US' })}</h1>;
```

### Text component

```jsx
import { Text } from '@particular.cloud/i18n-react';

return (
  <h1>
    <Text textKey="landingPageTitle" language="en-US" />
  </h1>
);
```

Welcome to easy string management! 🌟

**Note:** `useText` and `Text` can be used interchangeably.

---

## Configuration

`i18n-react` supports a wide variety of configuration options. You can adapt our i18n package to your needs.

In most cases, there are only a few fields that should be interesting.
The [I18nProvider](/documentation/developers/v1/react-api#i18nprovider) takes a config prop of type [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig).

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider
    config={{
      token: '<read-only-token>',
      acceptLanguage: 'en-US,en;q=0.9,de-DE;q=0.8,de;q=0.7',
      defaultLanguage: 'en-US',
      enableWebsocket: true,
      onKeyNotFound: 'generatePlaceholder',
    }}
  >
    <App />
  </I18nProvider>
);
```

### Accept-Language header

**Note:** You don't even need to specify the language in the `useText` function call or `Text` component when you make use of the `defaultLanguage` and `acceptLanguage` configuration options!

```jsx
import { useText } from '@particular.cloud/i18n-react';

return <h1>{useText({ key: 'landingPageTitle' })}</h1>;
```

### Changes in real-time

**Note:** You can also pass your read-only token to the `I18nProvider` provider and active the `enableWebsocket` option to enable real-time updates of your translations! 🔥🔥🔥

Navigate to your settings page of your Particular.Cloud project and click "Publish" to trigger and update of your translations! 🚀

---

## Optional: VS Code extension

If you are using VS Code, get yourself the Particular.Cloud [VS Code extension](https://marketplace.visualstudio.com/items?itemName=particular-cloud.particular-cloud) to add some magic to your development workflow!

Read more about it in our [VS Code documentation](/documentation/developers/v1/vscode)!
