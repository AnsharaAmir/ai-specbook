---
sidebar_position: 2
---

# Chapter 2: Docusaurus Setup & Markdown Basics

Welcome to Chapter 2! In this chapter, we'll get our hands dirty and set up the foundation for our AI textbook. We'll install Docusaurus, a fantastic tool for building websites like this one, and then we'll learn the basics of Markdown, the language you'll use to write your content.

## Part 1: Setting Up Your Docusaurus Website

Docusaurus makes it incredibly simple to create a beautiful, feature-rich website. Let's walk through the setup process step-by-step.

### Step 1: Your Only Prerequisite: Node.js

The only tool you need to have installed is [Node.js](https://nodejs.org/en/download/). Docusaurus is built on top of it.

To check if you have it, open your terminal (PowerShell on Windows, Terminal on Mac) and run:

```bash
node -v
```

If you see a version number (e.g., `v20.11.0`), you're ready! If not, please download and install it from the official website.

### Step 2: Create Your Docusaurus Site

Now for the magic. In your terminal, navigate to where you want to create your project and run this single command:

```bash
npx create-docusaurus@latest my-ai-book classic
```

Let's break that down:
- `npx`: A tool that comes with Node.js to run commands.
- `create-docusaurus@latest`: The command to create a new Docusaurus site, using the latest version.
- `my-ai-book`: The name of the folder for your new project. You can change this!
- `classic`: This tells Docusaurus to use the "classic" template, which gives us a blog, docs, and pages out of the box.

### Step 3: Start Your Website

Once the command finishes, navigate into your new project folder:

```bash
cd my-ai-book
```

And start the local development server:

```bash
npm run start
```

Your new website is now running! Open a web browser and go to **http://localhost:3000** to see it.

:::tip Live Reloading is Awesome!
Try editing one of the files in the `docs` folder (like `intro.md`). As soon as you save, you'll see the website update automatically.
:::

## Part 2: Writing with Markdown

Markdown is a simple way to format plain text that gets converted into HTML. It's the standard for writing documentation and is what you'll use for all your content in Docusaurus.

### Headings

You create headings using the `#` symbol. More `#`s mean a smaller heading.

```markdown
# This is a Heading 1 (like a chapter title)
## This is a Heading 2 (like a section)
### This is a Heading 3 (a sub-section)
```

### Text Formatting

- `*This text will be italic*` -> *This text will be italic*
- `**This text will be bold**` -> **This text will be bold**
- `~~This text will be strikethrough~~` -> ~~This text will be strikethrough~~

### Lists

**Unordered List:**
```markdown
* Item 1
* Item 2
  * A nested item
```

**Ordered List:**
```markdown
1. First item
2. Second item
3. Third item
```

### Links and Images

**Link:**
```markdown
[Visit the Docusaurus website](https://docusaurus.io/)
```

**Image:**
```markdown
![Docusaurus Logo](/img/docusaurus.png)
```
*(This assumes you have an image named `docusaurus.png` in the `static/img` folder).*

### Code Blocks

Show off your code with triple backticks. You can also specify the language for syntax highlighting.

````markdown
```js
function hello() {
  console.log("Hello, Docusaurus!");
}
```
````

## Part 3: Docusaurus-Flavored Markdown

Docusaurus adds some extra, powerful features to standard Markdown. Our favorite is **Admonitions**.

### Admonitions

Admonitions are a great way to highlight important information.

**Note:**
```markdown
:::note
This is a helpful piece of information.
:::
```

**Tip:**
```markdown
:::tip
Here's a pro-tip to make your life easier.
:::
```

**Caution:**
```markdown
:::caution
Be careful with this, as it could have unexpected results.
:::
```

There are also `info` and `danger` admonitions.

---

That's it for now! You've set up a Docusaurus site and learned the fundamentals of Markdown. In the next chapter, we'll start creating the actual content for our AI textbook.
