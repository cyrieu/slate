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

LangAPI exists to help developers rapidly translate and localize their apps for users worldwide. We automate translation between a huge number of languages, including Spanish, French, Chinese, and Russian. To make the developer experience as smoooth as possible, translating a string is as simple as wrapping it with a special function. Currently, we only support JavaScript and TypeScript codebases. You can choose to either use machine translations (Google Translate) for testing, or human translations for production.

# Getting Started

Follow the steps below to translate your first string in under 5 minutes!

## Sign Up

Before you start translating, you'll need to [sign up for an account](https://www.langapi.co/dashboard).

## Install the CLI and login

> In the terminal, install the "langapi" node module.

```shell--all
#!/usr/bin/bash

> npm install -g langapi
> langapi login
```

To make it easy to translate apps via terminal, we've built our own command-line interface (CLI). Our CLI includes three main commands: **init, push, and pull**.

- init - Initializes langapi in a project folder
- push - Pushes translation requests to our server
- pull - Fetches completed translations

To get help on these commands, add the --help flag.

## Initialization

> To create a brand-new project, use Create React App

```shell-all
npm install -g create-react-app
create-react-app sample_project
cd sample_project
```

We'll need an existing codebase to translate in this step. Feel free to use your personal blog or sideproject repo. If you don't have a project handy, you can create one with [Create React App (CRA)](https://facebook.github.io/create-react-app/).

> To initialize langapi, run langapi init in the root directory and specify your source directory with the --src flag

```shell-all
> langapi init --src src
```

Run **langapi init --src <SOURCE_DIRECTORY>** inside the project's **root directory** and follow the instructions.

**NOTE: The source directory should be where your actual code is. It is usually different than the root directory**.

We recommend using machine translations for testing out the workflow.

## Configuring target languages

> The langapiconfig.json contains your LangApi settings for the current project.
> Here, we've added Chinese (zh) as a target language for demonstration.

```json--all
{
  "originalLanguage": "en",
  "targetLanguage": ["zh"]
}
```

After running **langapi ini**, a new file **langapiconfig.json** will appear in your root directory. You can configure the behavior of LangAPI here, and you should check this file into your repository.

First, we'll need to set some target languages. To do this, open **langapiconfig.json** in any code editor, and in the targetLanguages property, add a comma-separated list of language encodings you want to translate to. By default, the original language is set to English (en).

[You can find a list of language encodings we support here](#language-codes).

## Mark string for translation

> We're using the App.js file in the src directory of our create-react-app

```javascript
import React, { Component } from "react";
import logo from "./logo.svg";
import "./App.css";
// Make sure to import tr from your LangClient
import { tr } from "./langapi/LangClient.js";

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          // Note the use of the tr function here
          <p>Watch us translate: {tr("Hello world!")}</p>
        </header>
      </div>
    );
  }
}
```

To wrap a string for translation, import the **tr** function from the newly generated LangClient file in your source directory. We demonstrate this using the sample create-react-app project we created previously.

## Push and Pull Translations

> To push translation requests, use 'langapi push'

```shell-all
> langapi push
```

> To pull completed translations, use 'langapi pull'

```shell-all
> langapi pull
```

You're almost there! We've marked the string for translation, but we need to actually translate it. To push your un-translated strings to our server to be translated, run **langapi push** in your **root directory** (where langapiconfig.json is).

After the command is finished, you'll receive a breakdown of your strings to be translated.

Since we're using machine translations in this demo, the translations are done instantly! Thanks Google. To pull them back into your repo, run **langapi pull** in your **root directory**.

## That's it

Congratulations! You've just translated your first string using LangAPI. We hope it was easy enough, and we'd love to hear your feedback. Shoot one of us an email at eric@langapi.co or peter@langapi.co and let us know what you think!

# LangApi Clients

> Language codes can be found on the Language Codes tab of this documentation.

```javascript
//LangClient.js
var LangClient = require("langapi")(
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
import LangTranslateClient from "langapi";

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

Wrap all of your desired strings with Lang API's tr function.

Adding support for variables inside the string is coming out in the next release - only plaintext strings are currently supported.

```javascript
// *.js, *.jsx
import { tr } from "./LangClient";

var translatedString = tr("Original string");
```

```typescript
// *.ts, *.tsx
import { tr } from "./LangClient";

const translatedString = tr("Original string");
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
```

```typescript
// *.ts, *.tsx
import tr from "./LangClient";

const translatedString = tr("Hello world!");
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
