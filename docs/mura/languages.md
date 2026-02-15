---
title: Languages
parent: Mura (Frontend)
nav_order: 3
---

# Languages

Languages and translations can be found in `/media/langs`.

## Translations

Each language should be stored as a json file with the language's ISO code as name, for example: `es-ES.json` (For spanish `es`, Spain variation `-ES`).\

## Configuration

Saved in `/media/langs/script.js`.\
**Default language:**\
Change the `default_lang` constant with the ISO code of the default language.\
**Available languages:**\
Change `languages` constant with the key being browser language and value the specific language file name.\
This allows similar but unsupported languages to still get one similar
```js
const languages = {
  'es-ES': 'es-ES',
  'es-419': 'es-ES',
  'es': 'es-ES'
};
```