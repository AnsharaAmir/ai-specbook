---
sidebar_position: 6
---

# Chapter 6: Urdu Translation and Multilingual Content

Our application is growing in features, but what about its audience? To reach a global community, supporting multiple languages is essential. In this chapter, we'll explore how to make our Docusaurus book available in both English and Urdu, and how Docusaurus provides a seamless way for users to toggle between languages.

## Docusaurus and Internationalization (i18n)

Fortunately, Docusaurus has a first-class internationalization (i18n) system designed to handle multilingual content out of the box. Instead of building a custom language toggle from scratch, we can leverage Docusaurus's powerful i18n capabilities.

The core principle is simple: for each additional language, you create a parallel set of translated document files. Docusaurus then handles the routing and provides a language dropdown for users to switch between them.

### 1. Configuring Docusaurus for Urdu

The first step is to tell Docusaurus about the languages we want to support. We do this in the `docusaurus.config.js` file.

We need to add an `i18n` configuration block, defining our default locale (English) and the new locale we want to add (Urdu).

```javascript
// docusaurus.config.js

module.exports = {
  // ... other config
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'ur'],
    localeConfigs: {
      en: {
        label: 'English',
        direction: 'ltr',
        htmlLang: 'en-US',
      },
      ur: {
        label: 'اردو',
        direction: 'rtl',
        htmlLang: 'ur-PK',
      },
    },
  },
  // ... other config
};
```

In this configuration:
- `defaultLocale`: The default language of our site.
- `locales`: An array of all supported language codes.
- `localeConfigs`: An object to provide specific metadata for each language. Notice for Urdu (`ur`), we've set the `label` to "اردو", the text `direction` to `rtl` (right-to-left), and the `htmlLang` attribute.

### 2. The Language Toggle Dropdown

Once you've configured the i18n system, Docusaurus automatically adds a language dropdown to your site's navigation bar. You don't need to build a custom toggle component.

This dropdown will now show "English" and "اردو", allowing users to switch between the two versions of your site seamlessly.

![Docusaurus Locale Dropdown](https://docusaurus.io/img/tutorial/localeDropdown.png)

### 3. Translating Your Content

Now for the translation itself. Docusaurus uses a command-line tool to initialize the directory structure for your new language. You would run:

```bash
npm run docusaurus write-translations -- --locale ur
```

This command creates a new directory at `i18n/ur`. Inside this directory, you'll find a structure that mirrors your original project, ready for you to add the translated files.

For our book, the translated chapters will go into `i18n/ur/docusaurus-plugin-content-docs/current/`.

#### Example: Translating a Chapter

Let's say we want to translate `docs/chapter-1.md`. We would create a corresponding file at `i18n/ur/docusaurus-plugin-content-docs/current/chapter-1.md`.

Here is a side-by-side example:

**`docs/chapter-1.md` (English)**
```markdown
---
sidebar_position: 1
---

# Chapter 1: The Vision

Welcome to the first chapter of our journey. Here, we will outline the grand vision for the AI-powered application we are about to build. Our goal is to create a smart assistant that can understand and process information from various sources.
```

**`i18n/ur/docusaurus-plugin-content-docs/current/chapter-1.md` (Urdu)**
```markdown
---
sidebar_position: 1
---

# باب ١: وژن

ہمارے سفر کے پہلے باب میں خوش آمدید۔ یہاں، ہم اس AI سے چلنے والی ایپلیکیشن کے عظیم وژن کا خاکہ پیش کریں گے جسے ہم بنانے جا رہے ہیں۔ ہمارا مقصد ایک ایسا سمارٹ اسسٹنٹ بنانا ہے جو مختلف ذرائع سے معلومات کو سمجھ اور اس پر کارروائی کر سکے۔
```

Notice that everything is translated, including the title. The front matter (like `sidebar_position`) remains the same to keep the structure consistent. The right-to-left text direction we configured earlier will ensure the Urdu text is rendered correctly.

## What's Next?

By integrating Docusaurus's i18n system, we've made our book accessible to a wider, multilingual audience. This robust system handles all the complexities of routing and UI, letting us focus on what matters most: the content.

In the next chapter, we'll move from documentation to deployment. We'll explore how to build and deploy our Docusaurus site, making our book and its translations available to the world.