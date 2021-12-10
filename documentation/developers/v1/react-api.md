---
title: Complete React API Documentation
description: Our React npm package supports static site generation (SSG), server-side rendering (SSR), and good-ol' fashioned client-side applications. This article documents the complete API of the @particular.cloud/i18n-react npm package.
---

# React (i18n-react) API Documentation

This article documents the complete API of the `@particular.cloud/i18n-react` npm package. If you are looking for an introduction, follow our [integration guide](/documentation/developers/v1/react).

The `@particular.cloud/i18n-react` npm package exposes a set of TypeScript types and all the React tools you need to integrate localization with Particular.Cloud into your react app!

It also exposes the underlying`i18n` object and an `api` object from the underlying `@particular.cloud/i18n-js` package. Please find the API specification of the `i18n` object [here](/documentation/developers/v1/javascript-api). You can use the `api` object to access the Particular.Cloud REST API directly. Find the JavaScript REST API documentation [here](/documentation/developers/v1/javascript-api#rest-api).

---

## `I18nProvider`

> Provider

`pun: Particularly Configured for React`

The `I18nProvider` context provider is used to configure `i18n-react`.

A simple setup looks like this:

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider config={{ defaultLanguage: 'en-US' }}>
    <App />
  </I18nProvider>
);
```

**Note:** You should only use the `I18nProvider` once in your application. The config is set globally.

**Props** 

- config (optional): [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig)

The config prop is optional but enables a fast variety of different configurations. A few examples follow!

---

### Defining a default language

You can pass a default language to your `I18nProvider`. This allows you to call the [`useText` hook](#usetext) and the [`Text` component](#text) without the `language` parameter.

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider config={{ defaultLanguage: 'en-US' }}>
    <App />
  </I18nProvider>
);
```

Now you can call `useText` and `Text` without a language:

```jsx
import { useText } from '@particular.cloud/i18n-react';

return (
  <main>
    <h1>{useText({ key: 'landingPageTitle' })}</h1>
    <p>
      <Text textKey="landingPageDescription" />
    </p>
  </main>
);
```

**Note:** `useText` and `Text` can be used interchangeably.

**🔥 Hot Tip:** If you are utilizing server-side rendering and you have access to the `Accept-Language` header of the request, you can also pass a `acceptLanguage` property together with the `defaultLanguage` to the `I18nProvider`. This will allow you to use the `acceptLanguage` to determine the language to use. This is the #mediaQuery approach to setting the language! Read more about the `acceptLanguage` property [here](/documentation/developers/v1/javascript-api#particularconfig).

---

### Real-time updates from Particular.Cloud

To enable real-time updates from Particular.Cloud, you have to set the `enableWebsocket` property to true. This establishes a websocket connection between your applications and Particular.Cloud. You can then proceed and use the "Publish" button on your project's settings page on Particular.Cloud to push changes in real-time! 🚀

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider config={{
      token: '<read-only-token>',
      enableWebsocket: true,
    }}>
    <App />
  </I18nProvider>
);
```

**Note:** You have to pass your read-only project token to fetch from Particular.Cloud. You can find more information about project tokens [here](/documentation/developers/v1).

---

## `I18nProviderProps`

> Type

The `I18nProvider` context provider component accepts a `config` prop.

```typescript
interface I18nProviderProps {
    config?: ParticularConfig;
}
```

Find information about the `ParticularConfig` type in the [JavaScript API documentation](/documentation/developers/v1/javascript-api#particularconfig).

---

## `useText`

> hook

`pun: No hard(coded) feelings - localization`

The useText hook let's you query your localized texts in your React application.

**Note:** You have to initialize the [`I18nProvider`](#i18nprovider) first before you can use the `useText` hook!

```jsx
import { useText } from '@particular.cloud/i18n-react';

const string = useText({ key: 'landingPageTitle', language: 'en' });
```

**Parameters**

- params: [TranslationParams](/documentation/developers/v1/javascript-api#translationparams)

The `TranslationParams` object can contain the following properties:

- key: string
- language: string (optional)
- values: `Record<string, string | string[]>` (optional)
- isolate: boolean (optional)

Let's say you created the following two values for the key "landingPageTitle" on Particular.Cloud:

- landingPageTitle: "Particularly awesome title" (US English)
- landingPageTitle: "Besonders toller Titel" (Germany German)

Querying for the right value is as easy as that:

```jsx
const string = useText({ key: 'landingPageTitle', language: 'en' });

console.log(string); // "Particularly awesome title"
```

The key is used to identify one text on Particular.Cloud. The `language` property is used to specify the language of the text.

---

### Set language once

You can also use the `acceptLanguage` and/or `defaultLanguage` properties of the [config object](#particularconfig) to set the language once globally for your application:

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider config={{ defaultLanguage: 'en' }}>
    <App />
  </I18nProvider>
);
```

And now you can call `useText` accross your application without the need to specify a language:

```jsx
// the defaultLanguage is used
const string = useText({ key: 'landingPageTitle' });

console.log(string); // "Particularly awesome title"
```

---

### Languages vs. localized languages

The preffered way to query languages is via locale (localized langauges), e.g. en-US, en-GB, de-DE, and so on. But this doesn't mean you can call `t` only with locales. You can also query by language code (langCode), e.g. en, de, and so on. In this case, we will fetch your default localized language for that language code.

Let's say you maintian both en-US and en-GB on Particular.Cloud.
On the languages overview page of Particular.Cloud, you set **en-US** as the **default** for en.

Query by **en** will then return your **en-US** translation texts:

Calling `useText({ key: 'landingPageGreeting', language: 'en'})` will now return your **en-US** translation texts.

---

### Arrays

On Particular.Cloud, you can add more text values for one key for each language.

- landingPageBtn: `["Log me in!", "Sign me in!", "Beam me up, Scotty!"]`

```jsx
const string = useText({ key: 'landingPageBtn', language: 'en' });
```

A value will get picked randomly from the possible options.

**🔥 Hot Tip:** For unit testing purposes, you can configure the property `onPickStringFromArray` in your [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig) to specify that only the first value should be used.

---

### Value population

Also common in i18n packages, you can pass a map of values that will get populated.

**Note:** We are not utilizing Mustache or any other templating language!

We are using a lightweight templating format that only supports simple mapping of values like so:

- "landingPageGreeting": `"Welcome back {username}!"`

```jsx
const values = {
  username: 'John Doe',
};
const string = useText({ key: 'landingPageBtn', language: 'en', values });

console.log(string); // "Welcome back John Doe!"
```

---

### Arrays and value population

Both your queried texts and population values can be of type array. Both will be picked randomly, allowing for a great variety of responses which is especially useful for voice applications.

- carStatusMsg: `["Your {carname} is in perfect conditions.", "Your {carname} is doing great!"]`

```jsx
const values = {
  carname: ['Model S', 'Tesla', 'S'],
};
const string = useText({ key: 'carStatusMsg', language: 'en', values });

console.log(string)
// Will print one of the following:
// "Your Model S is in perfect conditions."
// "Your Tesla is in perfect conditions."
// "Your S is in perfect conditions."
// "Your Model S is doing great!"
// "Your Tesla is doing great!"
// "Your S is doing great!"
```

**🔥 Hot Tip:** For unit testing purposes, you can configure the property `onPickStringFromArray` in your [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig) to specify that only the first value should be used.

```jsx
import { I18nProvider } from '@particular.cloud/i18n-react';

return (
  <I18nProvider config={{ onPickStringFromArray: 'pickFirst' }}>
    <App />
  </I18nProvider>
);
```

And now only the first element will be picked every time:

```jsx
const values = {
  carname: ['Model S', 'Tesla', 'S'],
};
const string = useText({ key: 'carStatusMsg', language: 'en', values });

console.log(string);
// "Your Model S is in perfect conditions."
```

---

### `isolate`

> boolean

The `isolate` property is used to specify that the translation should be isolated. That means that you can specify a certain language and that language will not override the internal global language used by `i18n-js`. This is great if you want to display a certain language in your application, but you don't want to change the global language.

Read more about the `isolate` property [here](/documentation/developers/v1/javascript-api#t).

---

## `Text`

> Component

`pun: No strings attached - manage localized text`

The Text component let's you query your localized texts in your React application.

```jsx
import { Text } from '@particular.cloud/i18n-react';

function Component() {
  return (
    <Text textKey="landingPageTitle" language="en">
  )
}
```

**Note:** You have to initialize the [`I18nProvider`](#i18nprovider) first before you can use the `Text` component!

**Props**

> [TextProps](#textprops)

- textKey: The key of the text you want to query.
- language: The language you want to query, can be both a locale (en-US, de-DE, ...) or a language code (en, de, es, ...).
- values: A map of values that will get populated into your text.
- isolate: If true, the text will not override the global language used by `i18n-js`.

Under the hood, the `Text` component utilizes the [useText](#usetext) hook to query your localized text.

### Why a component?

You cannot use hooks in class components. This is why we also offer a component to query your localized text. Feel free to use the `Text` component or the `useText` hook based on your own preferences!

### `textKey` vs `key`

Be aware that the `Text` component requires a `textKey` property. This differs from the `useText` hook which requires a `key` property.

**But why?**

The `key` property is reserved by React for the management of lists and cannot be used as a custom prop name for components.

Worry not, `i18n-react` will throw an error if you forget the `textKey` property!

**Note:** `useText` and `Text` can be used interchangeably.

---

## `TextProps`

> Type

The [`Text` component](#text) requires props of type `TextProps`.

```typescript
export interface TextProps {
    textKey: string;
    language?: string;
    values?: Record<string, string | string[]>;
    isolate?: boolean;
}
```

**Note:** We cannot use the word `key` since it is reserved by React for managing lists

The `TextProps` type fulfills the same purpose as the `TranslationParams` type. Find more details about the properties in the [`TranslationParams` documentation](/documentation/developers/v1/javascript-api#translationparams).

---

## `useLanguage`

> hook

The `useLanguage` hook lets you quickly access the current language code or locale that is used by `i18n-react` in your application.

```jsx
import { useLanguage } from '@particular.cloud/i18n-react';

const { langCodeOrLocale } = useLanguage();
```

**Note:** The `langCodeOrLocale` property can also be undefined. For instance when you don't specify the `defaultLanguage` and `acceptLanguage` properties in your [ParticularConfig](/documentation/developers/v1/javascript-api#particularconfig) configuration.

```typescript
interface UseLanguageReturnValue {
    langCodeOrLocale: LangCodeOrLocale | undefined;
}
```

The `langCodeOrLocale` value changes based on the latest texts queried by `i18n-react`.

---

## `I18nContext`

> Context

Usually, you don't need to access the `I18nContext` directly. 

```typescript
interface I18nContext {
    langCodeOrLocale?: LangCodeOrLocale;
    published?: Date;
    config?: ParticularConfig;
}
```

The `langCodeOrLocale` property can be accessed by the[`useLanguage` hook](#uselanguage). It contains the latest language code or locale used by `i18n-react`. 

The `published` property denotes the last `Date` when the "Publish" event was received from Partiuclar.Cloud. This property is used internally to update all queried texts based on the websocket connection. You can read more about establishing real-time text updates in the [`I18nProvider` section](#i18nprovider).

The `config` object contains the `ParticularConfig` object that was passed to the `I18nProvider` component.

---

## `i18n`

> object

The `i18n` object is re-exported from `i18n-react` and provides all functionalities of the `i18n-js` package. Please find the API specification of the `i18n` object [here](/documentation/developers/v1/javascript-api).

---

## `api`

> object

You can use the `api` object to access the Particular.Cloud REST API directly. Find the JavaScript REST API documentation [here](/documentation/developers/v1/javascript-api#rest-api).