---
date: 2026-05-07
tags:
  - Internationalisation
  - i18n
  - Angular
  - Localisation
description: A doc to explain how to transform your site to display in multi-language with Angular's built-in i18n (mark text, extract, translate, configure locales, build, deploy).
Parent: "[[Angular]]"
---
## 📋 Table of Contents

- [1. How it works](#1-how-it-works)
- [2. Localise package](#2-localise-package)
- [3. Mark text for translation](#3-mark-text-for-translation)
- [4. Extract the source file](#4-extract-the-source-file)
- [5. Create the translation files](#5-create-the-translation-files)
- [6. Configure angular.json](#6-configure-angularjson)
- [7. Run and build per language](#7-run-and-build-per-language)
- [8. Use the locale at runtime](#8-use-the-locale-at-runtime)
- [9. Deployment](#9-deployment)
- [10. Pitfalls](#10-pitfalls)
- [11. Checklists](#11-checklists)

---

## 1. How it works

Angular's native i18n is **compile-time**: translations are injected **at build**, so you get **one complete app per language**, not a runtime switch.

```text
Templates (i18n)  ──ng extract-i18n──▶  portfolio.xlf   (source language)
                                             │ copy + translate
                                             ▼
                              portfolio.fr.xlf / portfolio.nl.xlf
                                             │ ng build
                                             ▼
                        dist/…/en-GB   dist/…/fr-BE   dist/…/nl-BE
```

| Term              | Meaning                                                            |
| ----------------- | ------------------------------------------------------------------ |
| **Source locale** | Language the text is written in (e.g. `en-GB`)                     |
| **Locale ID**     | Language + region code (`fr-BE`, `nl-BE`), injected as `LOCALE_ID` |
| **XLIFF (`.xlf`)**| XML translation file: one `<trans-unit>` per message               |
| **Message ID**    | Key of a message, auto-generated or set by you with `@@myId`       |

> ✅ Advantages: fastest runtime, native formats for dates, numbers, currencies.
> ❌ Drawback: switching language = **loading another version of the site** (page reload).

---

## 2. Localise package

To add the `@angular/localize` package, use the following command to update the `package.json` and TypeScript configuration files in your project.

```bash
ng add @angular/localize
```

It also adds `@angular/localize/init` to the `polyfills` of `angular.json` (needed for `$localize`):

```json
"polyfills": ["zone.js", "@angular/localize/init"]
```

---

## 3. Mark text for translation

### In templates: `i18n` attribute

```html
<h2 i18n>Experience</h2>

<!-- With meaning | description @@custom-id -->
<h2 i18n="site header|Title of the experience section@@experience.title">Experience</h2>
```

- **meaning | description**: context for the translator. The `meaning` is part of the ID computation.
- **`@@id`**: a **custom, stable ID**. Recommended: changing the English text won't break existing translations.

### Attributes: `i18n-<attribute>`

```html
<input placeholder="Your full name *" i18n-placeholder="@@contactForm.namePlaceholder">
<button aria-label="Toggle navigation menu" i18n-aria-label>Menu</button>
```

### Text without a wrapper element: `ng-container`

```html
<ng-container i18n>Hi, it's Antoine</ng-container>
```

### In TypeScript: `$localize`

```ts
this.toastr.success($localize`Message sent successfully`);

// With a placeholder and a custom ID
const msg = $localize`:@@toast.hello:Hello ${this.name}:name:`;
```

> ❌ Never write ``$localize`${'Message sent'}` ``: the text is passed as a **placeholder**, so it can't be translated. The text must be **in the template literal itself**.

### Plural and select (ICU expressions)

```html
<span i18n>
  Updated {minutes, plural, =0 {just now} =1 {one minute ago} other {{{minutes}} minutes ago}}
</span>

<span i18n>
  The status is {status, select, active {active} archived {archived} other {unknown}}
</span>
```

---

## 4. Extract the source file

```bash
ng extract-i18n --output-path src/assets/data/locales --out-file portfolio.xlf
```

| Option          | Role                                                       |
| --------------- | ---------------------------------------------------------- |
| `--format`      | `xlf` (XLIFF 1.2, default), `xlf2`, `xmb`, `json`, `arb`   |
| `--out-file`    | File name (default `messages.xlf`)                         |
| `--output-path` | Target folder                                              |

Result (source file):

```xml
<trans-unit id="contactForm.namePlaceholder" datatype="html">
  <source>Your full name *</source>
  <context-group purpose="location">
    <context context-type="sourcefile">src/app/pages/contact-me/contact-me.component.html</context>
  </context-group>
</trans-unit>
```

---

## 5. Create the translation files

1. **Copy** the source file, one per language: `portfolio.xlf` → `portfolio.fr.xlf`, `portfolio.nl.xlf`
2. Add a **`<target>`** after each `<source>`:

```xml
<trans-unit id="contactForm.namePlaceholder" datatype="html">
  <source>Your full name *</source>
  <target>Votre nom complet *</target>
</trans-unit>
```

3. Keep placeholders untouched: `<x id="INTERPOLATION" equiv-text="{{ name }}"/>` must stay in the target.

### When the text changes

`ng extract-i18n` **overwrites only the source file**, it does **not** update `fr` / `nl`.

1. Re-extract `portfolio.xlf`
2. Compare with the translated files (`git diff`, or a merge tool such as `xliffmerge`)
3. Add the new `<trans-unit>` in each language and translate them
4. Remove the obsolete ones

---

## 6. Configure angular.json

Real example from the portfolio (source `en-GB`, translations `fr-BE`, `nl-BE`):

```json
"projects": {
  "Portfolio": {
    "i18n": {
      "sourceLocale": { "code": "en-GB", "baseHref": "/" },
      "locales": {
        "fr-BE": { "translation": "src/assets/data/locales/portfolio.fr.xlf", "baseHref": "/fr/" },
        "nl-BE": { "translation": "src/assets/data/locales/portfolio.nl.xlf", "baseHref": "/nl/" }
      }
    },
    "architect": {
      "build": {
        "configurations": {
          "production": { "localize": true },
          "fr": { "localize": ["fr-BE"] },
          "nl": { "localize": ["nl-BE"] }
        }
      },
      "serve": {
        "configurations": {
          "fr": { "buildTarget": "Portfolio:build:development,fr" },
          "nl": { "buildTarget": "Portfolio:build:development,nl" }
        }
      }
    }
  }
}
```

| Option                   | Role                                                                        |
| ------------------------ | --------------------------------------------------------------------------- |
| `sourceLocale`           | Language of the text in the code                                            |
| `locales.<id>.translation` | Path to the translation file                                              |
| `baseHref`               | URL prefix of the localized site (`/fr/`)                                   |
| `subPath`                | Output **folder** name (default: the locale ID). Set it equal to the URL prefix to simplify hosting |
| `localize`               | `true` = all locales, or a list (`["fr-BE"]`)                               |
| `i18nMissingTranslation` | `warning` (default), `error` (fail the build), `ignore`                     |

> 💡 Set `"i18nMissingTranslation": "error"` in `production` so a missing translation never reaches the site.

---

## 7. Run and build per language

```bash
# Dev server in a given language (ONE locale at a time)
ng serve --configuration=fr
ng serve --configuration=nl

# Production build: all locales
ng build --configuration production
```

Build output: **one folder per locale** (`en-GB`, `fr-BE`, `nl-BE`, or the `subPath` you set), each one a complete app.

> ⚠️ In `ng serve`, only the locale being served exists, so the language switcher links (`/fr/`, `/nl/`) **do not work** in dev. Test them on a production build.

---

## 8. Use the locale at runtime

### Read the current locale

```ts
import { inject, LOCALE_ID } from '@angular/core';

currentLocale = inject(LOCALE_ID);   // 'en-GB' | 'fr-BE' | 'nl-BE'
```

### Language switcher

Each language is a **separate build**, so the switcher is a plain link (full page load), not a router navigation:

```html
<a href="/">English</a>
<a href="/fr/">French</a>
<a href="/nl/">Dutch</a>
```

### Dates, numbers, currencies

Built-in pipes follow the locale automatically (the locale data is loaded for each localized build):

```html
{{ date | date:'longDate' }}      <!-- 28 septembre 2026 (fr-BE) -->
{{ price | currency:'EUR' }}
```

### Content that is not in templates (JSON data)

Data files can be split per locale and loaded with `LOCALE_ID`:

```ts
@Injectable({ providedIn: 'root' })
export class DataService {
  private http = inject(HttpClient);
  private locale = inject(LOCALE_ID);

  getData<T>(): Observable<T> {
    return this.http.get<T>(`./assets/data/${this.locale}.json`).pipe(
      catchError(() => this.http.get<T>('./assets/data/en-GB.json'))   // fallback = source locale
    );
  }
}
```

---

## 9. Deployment

Each locale lives under its own URL prefix, and the server must serve the **right `index.html`** for each one (SPA fallback per locale). Example with **nginx**:

```nginx
location /fr/ { try_files $uri $uri/ /fr/index.html; }
location /nl/ { try_files $uri $uri/ /nl/index.html; }
location /    { try_files $uri $uri/ /index.html; }
```

- The folder names must match the URLs: use **`subPath`** in `angular.json`, or map the folders on the server.
- Bonus: redirect visitors according to the `Accept-Language` header at server level.

---

## 10. Pitfalls

| Problem                                         | Cause / fix                                                              |
| ----------------------------------------------- | ------------------------------------------------------------------------ |
| Text stays in English in one language           | Missing `<target>` → use `i18nMissingTranslation: "error"`               |
| `$localize` text never translated               | Text passed as `${'...'}` placeholder → put it in the literal itself     |
| Translations lost after editing the text        | Auto-generated ID changed → use custom IDs (`@@id`)                      |
| New strings absent from `fr` / `nl`             | `ng extract-i18n` only updates the source file → sync manually           |
| Language switcher broken in `ng serve`          | Only one locale is served in dev → test on a production build            |
| 404 on `/fr/` after deployment                  | No fallback rule per locale → see [deployment](#9-deployment)            |
| Fallback JSON file not found                    | Fallback name must match the source locale (`en-GB.json`, not `en.json`) |
| Build slower / heavier                          | One build per locale → use `fr` / `nl` configurations during development |

---

## 11. Checklists

### Add a new text

- [ ] Add `i18n` (with `@@id`) in the template or use `$localize`
- [ ] `ng extract-i18n`
- [ ] Add the new `<trans-unit>` in each `portfolio.<lang>.xlf` and translate
- [ ] Check with `ng serve --configuration=fr`

### Add a new language

- [ ] Copy `portfolio.xlf` → `portfolio.<lang>.xlf` and translate everything
- [ ] Add the locale in `i18n.locales` (`translation`, `baseHref`)
- [ ] Add the `build` and `serve` configurations for it
- [ ] Add a JSON data file `<locale>.json` if data is split per locale
- [ ] Add the link (and flag) in the language switcher
- [ ] Add the server rule (`/<lang>/`)

---

## 📚 Ressources

- [Angular i18n guide](https://angular.dev/guide/i18n)
- [Add the localize package](https://angular.dev/guide/i18n/add-package)
- [Prepare the component for translation](https://angular.dev/guide/i18n/prepare)
- [Work with translation files](https://angular.dev/guide/i18n/translation-files)
- [Merge translations into the app](https://angular.dev/guide/i18n/merge)
- [Deploy multiple locales](https://angular.dev/guide/i18n/deploy)
- [ICU expressions](https://unicode-org.github.io/icu/userguide/format_parse/messages/)
