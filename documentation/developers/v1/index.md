---
title: Technical Documentation
description: Particular.Cloud can be integrated easily with your applications. We provide npm packages for React and JavaScript and a VS Code extension.
---

# Technical Documentation

Particular.Cloud can be integrated easily with your applications. We offer SDKs for JavaScript and React to integrate Particular.Cloud into your products and services.

Our command-line interface (CLI) further integrates your app with the data from Particular.Cloud. The CLI is a great way to integrate Particular.Cloud with existing i18n workflows and SDKs.

Our VS Code extension helps you to quickly refactor your code to i18n translation calls. You can upload your strings right from VS Code to your project on Particular.Cloud!

---

## Integrations

- [JavaScript](/documentation/developers/v1/javascript)
- [React](/documentation/developers/v1/react)
- [CLI](/documentation/developers/v1/cli)
- [VSCode](/documentation/developers/v1/vscode)
- [REST](/documentation/developers/v1/rest)

You can find more [integreation examples and demo applications](https://github.com/Particular-Cloud/particular-integration-examples) on GitHub.

---

## Missing an integration?

We are working on supporting more ecosystems!

Let us know what you are missing at [@particular.cloud](https://twitter.com/CloudParticular) on Twitter.

In the meantime, maybe our [REST API](/documentation/developers/v1/rest) can help you solve your use case?

## Create a project

Start by creating a project on [Particular.Cloud](https://particular.cloud/).

Head over to [Particular.Cloud](https://particular.cloud/) to create your first project! [Create an account](https://particular.cloud/auth/signup) or [login into your excisting account](https://particular.cloud/auth/login). 

<img src="https://s3.us-west-1.amazonaws.com/particular.cloud/particular-integration-examples/create-project.png" alt="Create project on Particular.Cloud" width="400" style="border-radius: 0.375rem;">

Navigate to the dashboard and create a new project. 😎

<img src="https://s3.us-west-1.amazonaws.com/particular.cloud/particular-integration-examples/create-react-app/add-languages.png" alt="Add languages on Particular.Cloud" width="400" style="border-radius: 0.375rem;">

Next, let's add some languages to the newly-created project!

<img src="https://s3.us-west-1.amazonaws.com/particular.cloud/particular-integration-examples/create-react-app/create-text.png" alt="Create text on Particular.Cloud" width="400" style="border-radius: 0.375rem;">

Make sure to also create a few texts. Click on the `View Texts` button for one of your languages. You can also start translating the texts to other languages already.

<img src="https://s3.us-west-1.amazonaws.com/particular.cloud/particular-integration-examples/create-react-app/texts.png" alt="Texts list on Particular.Cloud" width="800" style="border-radius: 0.375rem;">

Awesome! 🥳 We are all set!

## Development configuration

### VS Code extension

Are you integrating our React or JavaScript npm packages? If you are using VS Code, get yourself the Particular.Cloud [VS Code extension](https://marketplace.visualstudio.com/items?itemName=particular-cloud.particular-cloud) to add some magic to your development workflow!

### Token creation

Any form of communication between an application and Particluar.Cloud requires you to create a project token.

We distinguish between two different types of tokens:

- read-only token
- write-access token

---

### Read-only token

Our CLI tool requires a read-only token to fetch your texts from Particular.Cloud. Additionally, you can configure our SDKs to use a read-only token to connect to Particular.Cloud to fetch texts and runtime (even in real-time 🔥).

Generating new tokens is easy: On the [developer portal](https://particular.cloud/dashboard/projects), navigate to the project settings page, and create a read-only token.

`video: https://vimeo.com/575404572`

**Note:** You can commit your **read-only** tokens to public repositories and to your client-side applications without fear.

#### Unsure how to use the read-only token?

Find more information based on your use case:

- [JavaScript](/documentation/developers/v1/javascript)
- [React](/documentation/developers/v1/react)
- [CLI](/documentation/developers/v1/cli/)
- [VSCode](/documentation/developers/v1/vscode/)
- [REST](/documentation/developers/v1/rest/)

**Note:** Make sure to delete unused tokens to avoid leaking your tokens!

---

### Write-access token

Write-access token enable the creation of new texts right from your VS Code editor. Download the Particular.Cloud [VS Code extension](https://marketplace.visualstudio.com/items?itemName=particular-cloud.particular-cloud)) and try it out! It will speed up your localization process immensely! Refactoring applications to integrate i18n was never easier!

**❗ IMPORTANT:** Write-access token have write-access to your Particular.Cloud project. You shoulder **never** commit your write-access token to a public repository or share it with anyone.

**Agreed?** Great!

Let's add a `.particularrc.json` file to our project root folder:

```json
  {
    "token": "<write-access-token>",
    "defaultLanguage": "en-US"
  }
```

**❗ IMPORTANT:** Hide your secret write-access token by adding the following to your `.gitignore` file:

```bash
# hide write-access token
.particularrc.json
```

Next, go back to your browser window and navigate to the settings page of your [Partiuclar.Cloud](https://particular.cloud) project and create a write-access token.

`video: https://vimeo.com/650518749`

Did you copy the token to your clipboard? Awesome! Let's replace `<write-access-token>` in the `.particularrc.json` file with your write-access token.

And your VS Code extension is ready to go! 🚀

