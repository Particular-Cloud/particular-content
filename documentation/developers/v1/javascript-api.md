---
title: Complete JavaScript API Documentation
description: Our JavaScript npm packages is tailored towards a fast variety of different use cases. This article documents the complete API of the @particular.cloud/i18n-js npm package.
---

# JavaScript (`i18n-js`) API Documentation

This article documents the complete API of the `@particular.cloud/i18n-js` npm package. If you are looking for an introduction, follow our [integration guide](/documentation/developers/v1/javascript).

The `@particular.cloud/i18n-js` npm package exposes a set of TypeScript types, an `i18n` object to manage localization in your application, and an `api` object to access the Particular.Cloud REST API directly.

Find the JavaScript REST API documentation [here](#rest-api).

---

## `ParticularConfig`

> Type ParticularConfig

`pun: Particularly Configured`

You can configure `i18n-js` by passing a config object of type `ParticularConfig` to the [init](#init) function.

You can also add and alter the current configuration by calling [addToConfig](#addtoconfig) later on.

All properties are optional but most have a default value that you need to override if you want to change it.

### Properties

- token
- acceptLanguage
- defaultLanguage
- onChangeDefaultLanguage
- onChangeLanguage
- onChangeConfig
- onLeftOverTemplateSyntax
- onPickStringFromArray
- onKeyNotFound
- onError
- enableWebsocket
- onPublish
- activeLanguagesOnly
- updateStrategy
- keyExpiresIn
- customCache

---

### `token`

> string

**Default Value:** undefined

Your read-only project token. Only required if you want to fetch data from Particular.Cloud in your application. 

Navigate to the settings page of your project and create a read-only token. Find more information about how to create a token in the [developer documentation](/documentation/developers/v1).

---

### `acceptLanguage`

> string

**Default Value:** undefined

Instead of passing a locale to each `t` function call, you can specify the `acceptLanguage` and `defaultLanguage` config options.

Particular.Cloud will automatically find the best fitting supported language of your application for the specified `acceptLanguage` value or fall back to the `defaultLanguage` value.

`acceptLanguage` follows the [`Accept-Language` HTTP header specification](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language).

You can switch the accepted languages by calling [setAcceptLanguage](#setacceptlanguage).

---

### `defaultLanguage`

> string

**Default Value:** undefined

A string representing the default language. This language will be used as a global fallback if the user's language is not supported or you don't specify a language in your queries.

Can be of type languageCode (en, de, ...) or locale (en-Us, en-GB, de-DE, ...)

You can switch the default language by calling [setDefaultLanguage](#setdefaultlanguage).

---

### `onChangeDefaultLanguage`

> function

**Default Value:** undefined

A callback function that is called whenever the `init` or `addToConfig` functions are executed. 

---

### `onChangeLanguage`

> function

**Default Value:** undefined

A callback function that is executed whenever the global language is changed. Let's say you pass an `acceptLanguage` and `defaultLanguage` value to the `init` function. `i18n-js` will try to match the best fitting language of your application for the specified `acceptLanguage` value or fall back to the `defaultLanguage` value. The global language changes based on requests to Particular.Cloud or local queries of `@particular.cloud/texts` (that's where we store your translations locally when you use our [CLI tool](/documentation/cli)).

This callback is useful if you determine the user's language based on an `Accept-Language` header but want to display the currently used language back to the user in your application.

---

### `onChangeConfig`

> function

**Default Value:** undefined

A callback function that is called whenever the `init` or `addToConfig` functions are executed. 

---

### `onLeftOverTemplateSyntax`

> 'ignore' | 'warn' | 'throw'

**Default Value:** 'warn'

We implement a simple tempalting engine, which enables you to pass variables to your `t` function calls that will populate placeholders in your translations.

```javascript
import i18n from '@particular.cloud/i18n-js';

// "greeting": "Hello {name}!"
const str = i18n.t({ key: 'greeting', values: { name: 'John' } });
console.log(str); // Hello John!
```

The `onLeftOverTemplateSyntax` property determinse what should if you missed a value in your `values` object:

- ignore: for we ignore left-over template syntax after populating values
- warn: for console.warn
- throw: for throw Error

---

### `onPickStringFromArray`

> 'pickRandom' | 'pickFirst'

**Default Value:** 'pickRandom'

You can create several values of the same language for one translation key on Particular.Cloud. This is especially useful when you want to add variety to your texts (e.g. for voice applications).

The `onPickStringFromArray` property determines how `i18n-js` should pick a value from the array of values:

- random: for production experience
- pickFirst: great for snapshot testing

---

### `onKeyNotFound`

> 'generatePlaceholder' | 'warn' | 'throw'

**Default Value:** 'warn'

What should happen if a queried key is not found?

```javascript
import i18n from '@particular.cloud/i18n-js';

// key "greeting" is not specified in your project
const str = i18n.t({ key: 'greeting' });
```

- generatePlaceholder: will return a mock value for testing
- warn: for console.warn
- throw: for throw Error

**Note:** `generatePlaceholder` only works if you fetch from Particular.Cloud. We do not store mock values locally, so you have to supply a `token` and specify the `updateStrategy` property.

---

### `onError`

> 'warn' | 'throw'

**Default Value:** 'warn'

What should happen if a request throws (e.g. token access denied)?

- warn: for console.warn
- throw: for throw Error

**Note:** Configuration errors will always throw an error. This option only handles runtime errors.

---

### `enableWebsocket`

> boolean

**Default Value:** false

The `enableWebsocket` property registers a websocket connection to listen to publish events from Particular.Cloud.

Navigate to the settings page of your project on [Particular.Cloud](https://particular.cloud) and scroll down to the "Applications" section. Smash the "Publish" button to broadcast a publish event to all of your applications.

**Note:** You need to supply a `token` to use this feature.

---

### `onPublish`

> function

**Default Value:** undefined

The `onPublish` callback is triggered when the webhook receives an publish event.

**Note:** You need to set the `enableWebsocket` property to `true` to use this feature.

---

### `activeLanguagesOnly`

> boolean

**Default Value:** undefined

Should we only consider `active` languages of your Particular.Cloud project? You can activate and deactivate languages in your project's language overview.

You can set an language to active once you are confident in your translations. 

- true: only active languages will be considered/returned.
- false: all languages in the project will be considered/returned.

**Note:** For a good developer experience, try: `activeLanguagesOnly: process.env.NODE_ENV === 'production'`. This will return all languages in development (for testing) and only active languages in production.

---

### `updateStrategy`

> 'never' | 'fetchOnExpired'

**Default Value:** 'never'

There are two ways to query your translations with `i18n-js`. You can fetch your translations during build time via our CLI tool or you can fetch them on demand. You can also do both together and utilize our caching system and real-time updats via our websocket connection. 

- never: If you don't want to fetch your translations on demand, you can set this property to `never`.
- fetchOnExpired: This will query your translations locally first and if they are not found or expired, we will fetch them from Particular.Cloud.

**Note:** The `t` function will always return synchronously and only fetches asynchronously if the `updateStrategy` is set to `fetchOnExpired` to update the cache.

---

### `keyExpiresIn`

> number (milliseconds)

**Default Value:** 86400000 (one day)

When (in miliseconds) should the cached key values expire?

Used in combination with the `fetchOnExpired` option. 

---

### `customCache`

> [CustomCache](#customcache)

**Default Value:** undefined

Pass an object of type `CustomCache` if you want to manage string caching yourself, e.g. to store strings in a redis cache.

Used in combination with the `fetchOnExpired` option. 

---

## `init`

> function

`pun: Particularly Configured`

Call the `init` function once to initialize `i18n-js`:

```javascript
const i18n = require('@particular.cloud/i18n-js');

i18n.init();
```

### Parameters

- config: [ParticularConfig](#particularconfig) (optional)

You can configure `i18n-js` by passing a config object of type [ParticularConfig](#particularconfig) to the `init` function.

```javascript
const i18n = require('@particular.cloud/i18n-js');

const isProd = process.env.NODE_ENV === 'production';
const handleVia = isProd ? 'throw' : 'warn';

const config = {
    token: '<read-only-token>',
    onLeftOverTemplateSyntax: handleVia,
    onKeyNotFound: handleVia,
    onError: handleVia,
    enableWebsocket: true,
    activeLanguagesOnly: isProd,
    updateStrategy: 'fetchOnExpired',
};

i18n.init(config);
```

You can also add and alter the current configuration by calling [addToConfig](#addtoconfig) later on.

**Note:** Calling `init` multiple times will overwrite the current configuration and reset unset values to their default values.

---

## `addToConfig`

> function

`pun: Particularly Configured`

You can add and alter the current configuration by calling `addToConfig`. The function expect a config object of type [ParticularConfig](#particularconfig).

### Parameters

- config: [ParticularConfig](#particularconfig)

**Note:** You have to call [init](#init) before you can add to config.

```javascript
const i18n = require('@particular.cloud/i18n-js');

i18n.init({ defaultLanguage: 'en-US' });

// user selects German
i18n.addToConfig({ defaultLanguage: 'de-DE' });
```

---

## `t`

> function

`pun: No hard(coded) feelings - localization`

The t (translate/text) function is how you query for strings.

**Note:** You have to call [init](#init) first before you can use the `t` function!

```javascript
import i18n from '@particular.cloud/i18n-js';

const string = i18n.t({ key: 'title', language: 'en-US' });
```

As you can see, our `t` function returns immediately. Depending on your configuration, it can also fetch for strings from Particular.Cloud!

### Parameters

- params: [TranslationParams](#translationparams)
- callback: [AsyncCallback](#asynccallback) (optional)

The `TranslationParams` parameter contains the following properties:

- key: string
- language: string (optional)
- values: `Record<string, string | string[]>` (optional)
- isolate: boolean (optional)

Let's say you created the following two values for the key "landingPageTitle" on Particular.Cloud:

- landingPageTitle: "Particularly awesome title" (US English)
- landingPageTitle: "Besonders toller Titel" (Germany German)

Querying for the right value is as easy as that:

```javascript
const string = i18n.t({ key: 'landingPageTitle', language: 'en-US' });

console.log(string); // "Particularly awesome title"
```

The `key` property is used to identify one text on Particular.Cloud. The `language` property is used to specify the language of the text.

---

### Set language once

You can also use the `acceptLanguage` and/or `defaultLanguage` properties of the [config object](#particularconfig) to set the language once globally for your application:

```javascript
i18n.init({ defaultLanguage: 'de-DE' });

const string = i18n.t({ key: 'landingPageTitle' });

console.log(string); // "Besonders toller Titel"
```

---

### Languages vs. localized languages

The preffered way to query languages is via locale (localized langauges), e.g. en-US, en-GB, de-DE, and so on. But this doesn't mean you can call `t` only with locales. You can also query by language code (langCode), e.g. en, de, and so on. In this case, we will fetch your default localized language for that language code.

Let's say you maintian both en-US and en-GB on Particular.Cloud.
On the languages overview page of Particular.Cloud, you set **en-US** as the **default** for en.

Query by **en** will then return your **en-US** translation texts:

```javascript
const string = i18n.t({ key: 'landingPageTitle', language: 'en' });

console.log(string); // "Particularly awesome title"
```

---

### Arrays

On Particular.Cloud, you can add more text values for one key for each language.

- "landingPageBtn": `["Log me in!", "Sign me in!", "Beam me up, Scotty!"]`

```javascript
const string = i18n.t({ key: 'landingPageBtn', language: 'en-US' });
```

A value will get picked randomly from the possible options.

**🔥 Hot Tip:** For unit testing purposes, you can configure the property `onPickStringFromArray` in your [ParticularConfig](#particularconfig) to specify that only the first value should be used.

---

### Value population

Also common in i18n packages, you can pass a map of values that will get populated.

**Note:** We are not utilizing Mustache or any other templating language!

We are using a lightweight templating format that only supports simple mapping of values like so:

- "landingPageGreeting": `"Welcome back {username}!"`

```javascript
const values = {
  username: 'John Doe',
};
const string = i18n.t({ key: 'landingPageGreeting', language: 'en-US', values });

console.log(string); // "Welcome back John Doe!"
```

---

### Arrays and value population

Both your queried texts and population values can be of type array. Both will be picked randomly, allowing for a great variety of responses which is especially useful for voice applications.

- carStatusMsg: `["Your {carname} is in perfect conditions.", "Your {carname} is doing great!"]`

```javascript
const values = {
  carname: ['Model S', 'Tesla', 'S'],
};
const string = i18n.t({ key: 'carStatusMsg', language: 'en-US', values });

console.log(string);
// Will print one of the following:
// "Your Model S is in perfect conditions."
// "Your Tesla is in perfect conditions."
// "Your S is in perfect conditions."
// "Your Model S is doing great!"
// "Your Tesla is doing great!"
// "Your S is doing great!"
```

**🔥 Hot Tip:** For unit testing purposes, you can configure the property `onPickStringFromArray` in your [ParticularConfig](#particularconfig) to specify that only the first value should be used.

```javascript
i18n.init({ onPickStringFromArray: 'pickFirst' });

const values = {
  carname: ['model S', 'Tesla', 'S'],
};
const string = i18n.t({ key: 'carStatusMsg', language: 'en-US', values });

console.log(string); // "Your model S is in perfect conditions."
```

---

### `isolate`

> boolean

The `isolate` property is used to specify that the translation should be isolated. That means that you can specify a certain language and that language will not override the internal global language used by `i18n-js`. This is great if you want to display a certain language in your application, but don't want to change the global language.

```javascript
import i18n from '@particular.cloud/i18n-js';

i18n.init({ defaultLanguage: 'en-US' });

const string = i18n.t({ key: 'title' }); // will print title in English

const messageForSpanishUsers = i18n.t({ key: 'message', language: 'es-ES', isolate: true }); // will print a message in Spanish

const string = i18n.t({ key: 'title' }); // will print title in English
```

---

### `callback`

> [AsyncCallback](#asynccallback)

The `t` function can also be called with a callback. This is useful if you want to fetch the string from Particular.Cloud asynchronously and update a value once the string is fetched.

```javascript
import i18n from '@particular.cloud/i18n-js';

i18n.init({ token: '<read-only-token>', updateStratey: 'fetchOnExpired', defaultLanguage: 'en-US' });

let title = '';
function updateTitle(t) {
  // t is the string fetched from Particular.Cloud
  console.log(t);
  title = t;
}

t = i18n.t({ key: 'title' }, updateTitle); 

// will print cached title value
console.log(string);
```

---

## `fetchT`

> function

`pun: No strings attached - network requests`

You can also fetch directly from Particular.Cloud using the `fetchT` function. 

### Parameters

- params: [TranslationParams](#translationparams)

The `TranslationParams` parameter contains the following properties:

- key: string
- language: string (optional)
- values: `Record<string, string | string[]>` (optional)
- isolate: boolean (optional)

Find more information about the usage of the `TranslationParams` parameter in the [`t` function section](#t).

---

### Usage

```javascript
import i18n from '@particular.cloud/i18n-js';

i18n.init({ token: '<read-only-token>', defaultLanguage: 'en-US' });

// asynchronous alternative to the t function
const string = await i18n.fetchT({ key: 'title' });
```

**Note:** The `fetchT` function still makes use of the runtime cache to avoid unnecessary network requests.

--- 

## `TranslationParams`

> Type

`pun:  No strings attached - translations`

Both the `t` and `fetchT` functions take an object parameter of type `TranslationParams`.

```typescript
interface TranslationParams {
  key: string;
  language?: string;
  values?: Record<string, string | string[]>;
  isolate?: boolean;
}
```

An object of type `TranslationParams` can contain the following properties:

- key: string
- language: string (optional)
- values: `Record<string, string | string[]>` (optional)
- isolate: boolean (optional)

Learn more about how to use the `TranslationParams` parameter in the [`t` function section](#t).

---

## `AsyncCallback`

> Type

You can pass an async callback function to the `t` function as a second parameter.

```typescript
type AsyncCallback = (text: string) => any;
```

Learn more about how to use the `AsyncCallback` parameter in the [`t` function section](#t).

---

## `getDefaultLanguage`

> function

Returns the curret `defaultLanguage` property of the i18n object as previously set via `init` and `addToConfig`. 

**Note:** The `defaultLanguage` can also be `undefined` if no default language is set.

---

## `setDefaultLanguage`

> function

Set a new `defaultLanguage` to be used in your translation queries.

---

## `getAcceptLanguage`

> function

Returns the curret `acceptLanguage` property of the i18n object as previously set via `init` and `addToConfig`. 

**Note:** The `acceptLanguage` property can also be `undefined` if no default language is set.

---

## `setAcceptLanguage`

> function

Set a new `acceptLanguage` to be used in your translation queries.

---

## `getLangCodeOrLocale`

> function

Returns the currently set global language code or locale, which was used for the latest translation query.

The return value is of type string but can both be a language code (langCode) or a locale.

You can also set a `onLanguageChange` callback to the [`Particular.Config`](#particularconfig) object to be notified when the language changes.

---

## `isInitialized`

> function

A helper function that returns `true` if the i18n object has been initialized. You initialize the i18n object by calling the `init` function.

---

## `CustomCache`

> Type

`pun: We love Johnny Cache`

```typescript
// e.g. en, en-US, en-GB, ...
type LangCodeOrLocale = string;
// e.g. landingPageTitle
type Key = string;
// e.g. "Hello World" for en-US
type TextValue = string | string[];
type Expired = boolean;
type RetrieveValue = [TextValue | undefined, Expired];

interface CustomCache {
    retrieve(key: Key, language: LangCodeOrLocale): Promise<TextValue | undefined | RetrieveValue>;
    put(key: Key, language: LangCodeOrLocale, value: TextValue): Promise<void>;
    keys?(): Promise<[LangCodeOrLocale, Key][]>;
}
```

On default, fetched texts are stored in a runtime cache. Each text is identified by the language and the localization key.

For most use cases, our built-in runtime cache and re-fetching should be good enough.

There might be some use cases where you want to manage caching yourself. For instance, if you run an Alexa application on an AWS lambda function. A runtime cache does not make as much sense as the lambda function might delete all runtime memory for each request forcing Particular.Cloud to re-fetch on each user request.

But fear not! You can implement a cache on your end and pass it to the `init` function!

```javascript
const i18n = require('@particular.cloud/i18n-js');

const customCache = {
  async retrieve(key, language) {
    // TODO fetch string from cache
    return 'Hello World';
  },
  async put(key, language, value) {
    // TODO store string value in cache
  },
};

const config = {
  token: '<read-only-token>',
  updateStratey: 'fetchOnExpired',
  customCache,
};

i18n.init(config);
```

**Note:** The `keyExpiresIn` property on [`ParticularConfig`](#particularconfig) is still considered for the runtime cache. But it will not take effect for your own impleay<<mentation. You have to manage key expiration on your own.

---

### `retrieve`

> function

The retrieve function should return a promise of the cached value.

The value can be an array of strings or a single string value. The `onPickStringFromArray` on [`ParticularConfig`](#particularconfig) is used to determine the strategy to pick a string from the array.

You can return `undefined` if the value for the key and language is not the in cache (not found). In this case, `i18n-js` will fetch from Particular.Cloud and call `put` to store the value in the cache.

Instead of returning `string`, `string[]`, or `undefined` directly, you can also return an object of type `RetrieveValue`.

---

### `RetrieveValue`

> Type

```typescript
type Expired = boolean;
type RetrieveValue = [string[] | string | undefined, Expired];
```

Returning an object of type RetrieveValue allows you more fine-grained control over the caching.

By returning `undefined`, you trigger a re-fetch. By returning a value (string/string[]), we assume the currently cached value is not expired yet.

`RetrieveValue` allows you to return a value but also to trigger a re-fetch for the next user request.

```javascript
return ['Hello World', true];
```

The code snippet above illustrates how you can signal that the currently cached value is expired.
In this case, 'Hello World' will be returned for the current `t` function call but a re-fetch happens async as well.
This ensures that the current user request can be handled via cache, while also updating the cached value for all future requests.

---

### `put`

> function

The put function should store the value for the key language pair into the cache and should return a promise of this action.

---

### `keys`

> function

Your custom cache can also provide a third `keys` function.
For most use cases, this is not needed.

The property `enableWebsocket` on [`ParticularConfig`](#particularconfig) can be set to `true` to enable websocket support. This enables real-time updates on publish.

Once you publish your changes on Particular.Cloud using the "Publish" button on your project's settings page,
a websocket event is sent to all your applications that
have a websocket running. Each application is then re-fetching
all strings from Particular.Cloud.

If you are hosting a custom cache and you are not implementing
the keys function, `i18n-js` will not be able to determine which strings need to be re-fetched. In this case, `i18n-js` will fetch **ALL** your texts for all languages from your project on Particular.Cloud and store them into your custom cache.

Nothing wrong about that! But in case you want to optimize that fetch, implement the `keys` function to let us know which strings have to be re-fetched.

This will reduce the payload size of the fetch and also reduce the content stored in your custom cache.

**Note:** If you are not using `enableWebsocket`, you can ignore the `keys` function.

The async `keys` function should return an array of tuples of all keys and their languages that are currently stored in the cache.

For instance, if the following values are stored in the cache:

-   landingPageTitle en-Us: "Particularly cool title"
-   landingPageGreeting de: "Guten Tag!"

A keys call should return the following array (as a promise):

```javascript
[
  ['landingPageTitle', 'en-Us'],
  ['landingPageGreeting', 'de'],
];
```

In this case, on publish, `i18n-js` will only fetch two text values from Particular.Cloud.

---

## `Language`

> Type

`pun: Randy Language`

`fetchAllLanguages` returns an array of type `Language`.

```typescript
interface Language {
    locale: string;
    countryCode: string;
    langCode: string;
    langName: string;
    langNameLocal: string;
    countryName: string;
    countryNameLocal: string;
    flag: string;
}
```

---

## `ProjectLanguage`

> Type

`fetchProjectLanguages` returns an array of type `ProjectLanguage`.

```typescript
interface Language {
    locale: string;
    countryCode: string;
    langCode: string;
    langName: string;
    langNameLocal: string;
    countryName: string;
    countryNameLocal: string;
    flag: string;
}

interface ProjectLanguage {
    isDefault: boolean;
    isActive: boolean;
    language: Language;
}
```

For instance, if you added three languages to your project on Particular.Cloud, e.g. 'de-DE', 'en-Us', and 'pt-BR', `fetchProjectLanguages`  will return:

```json
[
  {
    "isDefault": true,
    "isActive": true,
    "language": {
      "locale": "en-US",
      "langCode": "en",
      "langName": "English",
      "langNameLocal": "English",
      "countryCode": "US",
      "countryName": "United States of America",
      "countryNameLocal": "United States of America",
      "flag": "🇺🇸"
    }
  },
  {
    "isDefault": true,
    "isActive": true,
    "language": {
      "locale": "de-DE",
      "langCode": "de",
      "langName": "German",
      "langNameLocal": "Deutsch",
      "countryCode": "DE",
      "countryName": "Germany",
      "countryNameLocal": "Deutschland",
      "flag": "🇩🇪"
    }
  },
  {
    "isDefault": false,
    "isActive": false,
    "language": {
      "locale": "pt-BR",
      "langCode": "pt",
      "langName": "Portuguese",
      "langNameLocal": "Português",
      "countryCode": "BR",
      "countryName": "Brazil",
      "countryNameLocal": "Brasil",
      "flag": "🇧🇷"
    }
  }
]
```

**Note:** The `isDefault` property denotes if a language is set as the default language for a language code for the project.

**Note:** The `isActive` property lets you know if a language is currently active for the project.

---

## Text

> Type

`fetchTexts` returns an array of type `Text`. `fetchText` returns an object of type `Text`.

```typescript
interface Text {
    key: string;
    value: string | string[] | undefined;
    locale: Locale;
}
```

---

## TranslationRecords

> Type

Return value of the `fetchTranslationRecord` and `fetchTranslationRecords` functions.

```typescript
type Locale = string;
type Key = string;
type TextValue = string | string[];
type LanguageRecord = Record<Key, TextValue>;
type TranslationRecords = Record<Locale, LanguageRecord>;
```

**Example:**

```json
{
  "en-US": {
    "LandingPageTitle": "Particularly cool title",
    "LandingPageGreeting": ["Hello World!", "Hi there!"]
  }
}
```

---

## REST API

You can also access our REST API directly using the `api` object exposed by `i18n-js`.

```javascript
import { api } from '@particular/i18n-js';
```

**Note:** You have to call [init](#init) with your read-only token before you can interact with the REST API.

### Properties

`pun:  Who needs UIs anyways?`

- addTextKey
- fetchTexts
- fetchText
- queryTexts
- fetchTranslationRecord
- fetchTranslationRecords
- fetchAllLanguages
- fetchProjectLanguages

---

#### `addTextKey`

**Note:** `addTextKey` is the only POST request available via `i18n-js`. The function call requires a write-access token. Please treat your write-access token carefully! Please refer to the [general developer guide](/documentation/developers/v1#developmentconfiguration) for more information.

`addTextKey` lets you create a new key in your Particular.Cloud project.

```javascript
import { api } from '@particular/i18n-js';

// be cautious with your write-access token!
i18n.init({ token: 'write-access-token' });

async function main() {
  await api.addTextKey('lpMainTitle');
}
```

`addTextKey` can take three parameters:

- key: string
- languageCodeOrLocale (optional): string
- value (optional): string

---

**key**

> string

The key that will be created. `addTextKey` will return an error, if the key is already taken.

---

**languageCodeOrLocale (optional)**

> string

Only required if you want to pass an initial value for the key.

A language code or locale (en, de, en-US, de-DE, ...) that specifies the language of the passed value. If the locale is a language code (langCode), e.g. "en" or "de", the default language from the project for that language will be picked.

---

**value (optional)**

> string

Pass an optional value. The value will be saved for the locale to the new key. 

**🔥 Hot tip:** Use the value to add a note or initial value for your team to work with. It can be quite hard to understand what the key was supposed to mean without an initial demo value!

---

**Some Examples**

```javascript
import { i18n, api } from '@particular/i18n-js';

i18n.init({ token: 'read-only-token' });

async function main() {
  await api.addTextKey('lpDocDescription', 'en-US', 'Description on landing page regarding documentation');
  await api.addTextKey('lpDocBtn', 'en-US', 'Go to docs');
  await api.addTextKey('lpTeamTitle', 'en-US', 'TODO nice team title');
}
```

---

**Key naming conventions**

You can name your keys however you like. Anything that is supported by the general JSON schema is allowed.

We recommend the following naming convention for better placeholder support:

`[pageName][containerName][Title|Description|Label]`

For instance, name the landing page title of the documentation section: "lpDocTitle".

---

#### `fetchTexts`

> function

`pun:  Do it yourself!`

Fetches all text values from Particular.Cloud for a given language.

**Parameters**

- language: string (optional)

**Note:** The `language` parameter can be a locale or a language code (langCode), e.g. en, de, en-US, de-DE.

```javascript
import { i18n, api } from '@particular/i18n-js';

i18n.init({ token: 'read-only-token' });

const texts = await api.fetchTexts('en');
```

`fetchTexts` returns an array of [type `Text`](#text).

**Note:** To fetch all texts for all languages, just omit the language paramenter.

```javascript
// fetches all texts from all languages
const translationRecord = await api.fetchTexts();
```

---

#### `fetchText`

> function

Fetches one text from Particular.Cloud for a key language pair.

**Parameters**

- key: string
- language: string

**Note:** The `language` parameter can be a locale or a language code (langCode), e.g. en, de, en-US, de-DE.


```javascript
const translationRecord = await api.fetchText('title', 'en');
```

`fetchText` returns an object of [type `Text`](#text).

---

#### `queryTexts`

> function

Fetches a list of texts from Particular.Cloud based on an array of text queries. Each query is a tuple of the form `[language, key]`.

**Parameters**

- query: `TextQuery[]`

```typescript
type LangCodeOrLocale = string;
type Key = string;
type TextQuery = [LangCodeOrLocale | undefined, Key];
```

**Example**

```javascript
import { i18n, api } from '@particular/i18n-js';

i18n.init({ token: 'read-only-token', defaultLanguage: 'en' });

const texts = await api.queryTexts([
  ['en-US', 'lpDocDescription'],
  ['de-DE', 'lpDocBtn'],
  [undefined, 'lpTeamTitle'],
]);
```

If the `Locale` parameter is omitted, the `defaultLanguage` and `acceptLanguage` will be used instead, if provided.

`queryTexts` returns an array of [type `Text`](#text). 

---

#### `fetchTranslationRecord`

> function

`fetchTranslationRecord` fetches the application texts for a specific language from your Particular.Cloud project.

**Parameters**

- locale: string
- stringOnly: boolean (optional)

If `stringOnly` is set to `true`, each text value will be returned as a string. The property `onPickStringFromArray` in your [ParticularConfig](#particularconfig) is used to determine how to pick from the value array (if value is of type array).

The `fetchTranslationRecord` function returns an object of type [TranslationRecords](#translationrecords).

Example json return value:

```json
{
  "en-US": {
    "LandingPageTitle": "Particularly cool title",
    "LandingPageGreeting": ["Hello World!", "Hi there!"]
  }
}
```

---

#### `fetchTranslationRecords`

> function

`fetchTranslationRecords` fetches all application texts from your Particular.Cloud project.

**Parameters**

- stringOnly: boolean (optional)

If `stringOnly` is set to `true`, each text value will be returned as a string. The property `onPickStringFromArray` in your [ParticularConfig](#particularconfig) is used to determine how to pick from the value array (if value is of type array).

The `fetchTranslationRecords` function returns an array of type [TranslationRecords](#translationrecords).

Example json return value:

```json
[
  {
    "en-US": {
      "LandingPageTitle": "Particularly cool title",
      "LandingPageGreeting": ["Hello World!", "Hi there!"]
    }
  },
  {
    "de-DE": {
      "LandingPageTitle": "Richtig cooler Titel",
      "LandingPageGreeting": ["Hallo Welt!", "Hallo!"]
    }
  }
]
```

---

#### `fetchAllLanguages`

> function

`fetchAllLanguages` fetches **ALL** languages that are supported by Particular.Cloud.

```javascript
import { api } from '@particular/i18n-js';

const languages = await api.fetchAllLanguages();
```

Returns an array of [type `Language`](#language).

---

#### `fetchProjectLanguages`

> function

The `fetchProjectLanguages` function returns a list of all languages that are supported by **your project** on Particular.Cloud.

**Parameters**

- filter: 'all' | 'active' | 'default' (optional)

**Default Value:** 'active'

The `filter` parameter let's you specify what project languages you want to fetch.

- all: returns all your project languages
- active: returns all your project languages that are active
- default: return all project languages that are default languages for a language code

**Note:** You can change the active and default status of a language in the language overview page of your project on Particular.Cloud.

```javascript
import { api } from '@particular/i18n-js';

i18n.init({ token: 'read-only-token' });

const projectLanguages = await api.fetchProjectLanguages();
```

`fetchProjectLanguages` returns an array of [type `ProjectLanguage`](#projectlanguage).

**🔥 Hot Tip:** If you want to display all available languages to the user (e.g. in a language dropdown), `fetchProjectLanguages` based on your `NODE_ENV`. Fetch all languages in development mode and only active languages in production mode.

```javascript
import { api } from '@particular/i18n-js';

i18n.init({ token: 'read-only-token' });

// test all languages in development mode
// use active language in production mode
const filter = process.env.NODE_ENV === 'production' ? 'active' : 'all';

const projectLanguages = await api.fetchProjectLanguages(filter);
```
