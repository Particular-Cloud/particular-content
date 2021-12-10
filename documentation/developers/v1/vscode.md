---
title: VS Code Extension Documentation
description: Our VS Code extension will speed up your development workflow immensely! Create new texts right from your editor, refactor strings to translation calls for both JavaScript and React projects, and read the latest texts from Particular.Cloud by hovering over your localization keys!
---

# VS Code Extension Documentation

Particular.Cloud can be integrated easily in your dev workflow.
We provide a [VS Code extension](https://marketplace.visualstudio.com/items?itemName=particular-cloud.particular-cloud)
that makes working with Particular.Cloud and our SDKs a breeze!

---

## Features

- [key autocompletion](#key-autocompletion)
- [text creation](#text-creation)
- [string refactoring](#string-refactoring)
- [t function call snippet](#t-code-snippet)
- [useText snippet](#usetext-code-snippet)

---

## Installation

Use the VS Code [Extension Marketplace](https://code.visualstudio.com/docs/editor/extension-marketplace)
and search for Particular.Cloud. Click on install. The extension will be installed globally.

---

## Authentification

Any form of communication between an application and Particluar.Cloud requires you to create a project token for your Particular.Cloud project.

### Write-access token

Write-access token enable the creation of new texts right from your VS Code editor.

**❗ IMPORTANT:** Write-access token have write-access to your Particular.Cloud project. You shoulder **never** commit your write-access token to a public repository or share it with anyone.

**Agreed?** Great!

Let's add a `.particularrc.json` file to our project root folder:

```json
  {
    "token": "<write-access-token>",
    "defaultLanguage": "en-US"
  }
```

**Note:** The `defaultLanguage` property is used to display the latest texts from Particular.Cloud on hover over localization keys. It is also used when refactoring strings to i18n calls. So make sure to set it to the language you want to use (e.g. the language of your project).

**❗ IMPORTANT:** Hide your secret write-access token by adding the following to your `.gitignore` file:

```bash
# hide write-access token
.particularrc.json
```

Next, go back to your browser window and navigate to the settings page of your [Partiuclar.Cloud](https://particular.cloud) project and create a write-access token.

`video: https://vimeo.com/650518749`

Find more information about how to create tokens in the [developer documentation](/documentation/developers/v1).

Did you copy the token to your clipboard? Awesome! Let's replace `<write-access-token>` in the `.particularrc.json` file with your write-access token.

And your VS Code extension is ready to go! 🚀

---

## `defaultLanguage`

The default language should be set to the language that your code is currently working with. Let's say you have hard-coded strings in your project in German.

```json
  {
    "token": "<write-access-token>",
    "defaultLanguage": "de-DE"
  }
```

Setting the `defaultLanguage` property to German will make sure that the latest texts from Particular.Cloud are displayed in German when hovering over localization keys. 

Refactoring your hard-coded strings to translation calls will now store the strings in German in your Particular.Cloud project! ✅

---

## Key autocompletion

We support translation key autocompletion within our [t function](/documentation/developers/v1/javascript-api#t) and [useText hook](/documentation/developers/v1/react-api#usetext). Enjoy the real-time key IntelliSense! 🚀

Just start typing your key name and select the right key from the autocompletion dropdown! The dropdown also displays the latest texts from Particular.Cloud in your [default language](#defaultlanguage).

`video: https://vimeo.com/575404468`

**🔥 Hot tip:** Use the VS Code built-in key binding `keyhint: [cmd i](to trigger IntelliSense without typing)` to trigger the IntelliSense without typing!

---

## Text creation

You can create new keys right from the VS Code editor through the Command Palette (`keyhint: [cmd shift p](to open the Command Palette)`). Just start typing: `Particular.Cloud: Add new key to project` and follow the instructions in the Command Palette!

You might have noticed that we also display a "Create new key" action within the [key autocompletion](#key-autocompletion) dropdown. That's because we also support key creation in our [t function](/documentation/developers/v1/javascript-api#t) and [useText hook](/documentation/developers/v1/react-api#usetext).

Just type your new key name and click on "Create new key" in the autocompletion dropdown!

`video: https://vimeo.com/575404541`

**🔥 Hot tip:** Use the VS Code built-in key binding `keyhint: [cmd i](to trigger IntelliSense without typing)` to trigger the dropdown in case it does not show up!

---

## String refactoring

Refactor strings to i18n calls! Select the text in your editor that you want to refactor and hit the Command Palette (`keyhint: [cmd shift p](to open the Command Palette)`). Now just start typing: `Particular.Cloud: Refactor text to i18n call and add text with new key to project` and follow the instructions in the Command Palette!

Your strings are automatically sent to your Particular.Cloud project! 🚀

`video: https://vimeo.com/575404512`

**🔥 Hot tip:** Use our keybinding! Once you have selected your hard-coded string in your editor, press `keyhint: [cmd shift i](to trigger the refactor command)` to trigger the refactor command.

## `t` code snippet

The [t (translation/text) function](/documentation/developers/v1/javascript-api#t) from our `i18n-js` npm package is used to query your localized texts in the right language.

Use our `t` function call snippet to quickly add a `t` call to your code! 🚀

`video: https://vimeo.com/575404468`

## `useText` code snippet

The [useText hook](/documentation/developers/v1/react-api#usetext) from our `i18n-react` npm package is used to query your localized texts in the right language.

Use our `useText` hook snippet to quickly add new `useText` calls in your application! 🚀

`video: https://vimeo.com/575404435`

## Don't like the snippets?

Snippets can be really helpful if utilized often but can also come in the way and be annoying.

VS Code offers a wide variety of configurations to hide, sort, and manage snippets.

Please follow [this guide](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_can-i-remove-snippets-from-intellisense) to configure snippet visibility in VS Code.