---
title: REST Documentation
description: Particluar.Cloud offers an extensible REST API to read and write from and to your Particluar.Cloud project.
---

# REST Documentation

Please follow our [Swagger documentation](https://particular.cloud.com/api/v1/api-docs/) for more information.

You can also call our open-data (public) endpoints via [rapidapi](https://rapidapi.com/particularly.discrete.devs/api/localized-languages1).

---

We provide endpoints for

- **/languages**
- **/texts**

---

```bash
curl https://particular-cloud.com/api/v1/languages
```

---

**Note:** Our project-specific endpoints require an Authorization header and an valid read-only token.

```bash
curl https://particular-cloud.com/api/v1/languages
-H "Authorization: <read-only-token>"
```

---

### Read-only token

Read-only token are used to authenticate with Particular.Cloud.

Navigate to the settings page of your project and create a read-only token. Find more information about how to create a token in the [developer documentation](/documentation/developers/v1).

**Note:** You can commit your **read-only** tokens to public repositories and to your client-side applications without fear.

---

**Note:** All our endpoints are also available through our [JavaScript npm package](/documentation/developers/v1/javascript).

```javascript
import i18n from '@particular.cloud/i18n-js';

i18n.init({ token: '<read-only-token>' });
const supportedLanguages = await i18n.api.fetchProjectLanguages();
```
