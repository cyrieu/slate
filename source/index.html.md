---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - typescript

toc_footers:
  - <a href='https://www.langapi.co'>Sign Up for Lang API</a>

search: true
---

# Introduction

Welcome to Lang API! You can use our API and node module to make translation requests and received updated translations.

# Installation

First, we need to add the langapi node module to our project and ensure we have loaded the correct API key.

> In the terminal, install the "langapi" node module globally.

```shell--all
#!/usr/bin/bash

> npm install -g langapi
```

> To authorize, make sure your .env file contains the LANG_KEY environment variable.

```shell--all
# .env

LANG_KEY=LANG_API_KEY
```

> Make sure to replace `LANG_API_KEY` with your API key.

# Setup

Run **langapi init** inside the source directory to generate default config files, and set your initial and target languages inside of langconfig.json.

An additional file called LangConfig.js will be created, containing the client used to lookup translations. Export the tr function from this file to translate your strings.

```shell--all
#!/user/bin/bash

> langapi init
```

> A --src flag can be provided to specify the source directory. If no argument is provided, it defaults to "./src".

```json--all
// .lang/langconfig.json
{
  "originalLanguage": "en",
  "targetLanguage": [/* PUT YOUR REQUESTED LANGUAGES HERE */]
}
```

> Language codes can be found on the Language Codes tab of this documentation.

```javascript
//LangClient.js
var LangClient = require("langapi-client")(
  process.env.LANG_KEY,
  require("./.lang/translations.json"),
  require("./.lang/langconfig.json")
);

export function tr(phrase) {
  return LangClient.tr(phrase);
}
```

> You can generate a typescript Lang Client by adding the --ts flag.

```typescript
// LangClient.ts
import LangTranslateClient from "langapi-client";

const LangClient = new LangTranslateClient(
  process.env.LANG_KEY,
  require("./.lang/translations.json"),
  require("./.lang/langconfig.json")
);

export function tr(phrase: string) {
  return LangClient.tr(phrase);
}
```

# Preparing Texts For Translation

Wrap all of the strings you want translated with Lang API's tr function.

```javascript
// *.js, *.jsx
import { tr } from "./LangClient";

var translatedString = tr("Hello world!");

// translatedString (es): ¡Hola Mundo!
```

```typescript
// *.ts, *.tsx
import { tr } from "./LangClient";

const translatedString = tr("Hello world!");

// translatedString (es): ¡Hola Mundo!
```

# Interpolation

Interpolation allows you to add dynamic values to your translations, and will be escaped during the translation. You must put param() calls inside of tr() calls.

```javascript
// *.js, *.jsx
import { tr, param } from "./LangClient";

var ESCAPED_SITE = "Lang API";
var translatedString = tr("Welcome to " + param(ESCAPED_SITE) + "!");

// translatedString (es): ¡Bienvenido a Lang API!
```

```typescript
// *.ts, *.tsx
import { tr, param } from "./LangClient";

const ESCAPED_SITE = "Lang API";
const translatedString = tr("Welcome to " + param(ESCAPED_SITE) + "!");

// translatedString (es): ¡Bienvenido a Lang API!
```

# Request Translations

Search through your codebase for all requested strings and send them to our API endpoint for translation. The push function parses your codebase for tr(...) calls and requests all previously unrequested strings for you. Phrases are uniquely identified by string value, so two tr() calls on the same string will only generate one request.

```shell--all
#!/usr/bin/bash

> langapi push
```

> Make sure your .env file contains your Lang API key, which is used for authentication

# Receive Translations

The pull function gets all requested phrases that have been translated thus far caches the results inside of your translations.json, which is where tr(...) will fetch your translations.

```shell--all
#!/usr/bin/bash

> langapi pull
```

> Make sure your .env file contains your Lang API key, which is used for authentication

# Ready to go!

Once you've requested translations and cached the results in translations.json, you're all set to display localized strings with the tr(...) function. Strings that are not yet translated will default to the original language string.

```javascript
// *.js, *.jsx
var tr = require("./LangClient");

var translatedString = tr("Hello world!");

// translatedString (missing translation): Hello world!
```

```typescript
// *.ts, *.tsx
import tr from "./LangClient";

const translatedString = tr("Hello world!");

// translatedString (missing translation): Hello world!
```

#Language Codes

Here are the supported language codes you can use as originalLanguage and targetLanguages inside of langconfig.json.

| Code  | Language                |
| ----- | ----------------------- |
| fr-ca | French (Canada)         |
| da    | Danish                  |
| zh-tw | Chinese (Traditional)   |
| pt-br | Portuguese (Brazil)     |
| uk    | Ukrainian               |
| lt    | Lithuanian              |
| th    | Thai                    |
| dt    | Desktop                 |
| fi    | Finnish                 |
| ro    | Romanian                |
| is    | Icelandic               |
| es-la | Spanish (Latin America) |
| el    | Greek                   |
| hi    | Hindi                   |
| hu    | Hungarian               |
| fr    | French                  |
| it    | Italian                 |
| ko    | Korean                  |
| bg    | Bulgarian               |
| cs    | Czech                   |
| sk    | Slovak                  |
| pt    | Portuguese (Europe)     |
| en-gb | English (British)       |
| sv    | Swedish                 |
| id    | Indonesian              |
| de    | German                  |
| ms    | Malay                   |
| vi    | Vietnamese              |
| zh    | Chinese (Simplified)    |
| ge    | Generic                 |
| tl    | Tagalog                 |
| es    | Spanish (Spain)         |
| sr    | Serbian                 |
| ar    | Arabic                  |
| he    | Hebrew                  |
| nl    | Dutch                   |
| no    | Norwegian               |
| pl    | Polish                  |
| ja    | Japanese                |
| ru    | Russian                 |
| tr    | Turkish                 |
| en    | English                 |

If you want support for a language we don't have yet, reach out to us at support@langapi.co. We're always on call and happy to help!

# Changelog
