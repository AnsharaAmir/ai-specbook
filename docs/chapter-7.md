---
sidebar_position: 7
---

# Chapter 7: Integration and Deployment

We've come a long way. We've written specifications, designed architectures, and authored several chapters of our book. Now it's time to bring everything together, review our work, and share it with the world. This chapter will cover the final steps: integrating our content and deploying our Docusaurus site to GitHub Pages.

## Integrating and Reviewing Your Content

Before we deploy, it's crucial to ensure our book is coherent, consistent, and easy to read. When multiple chapters are written, sometimes by different people or at different times, inconsistencies can creep in. A final review pass is essential.

### Tips for a Final Content Review

Here is a checklist to guide you through the review process:

- **✅ Consistent Tone and Voice:** Read through all the chapters. Does the book sound like it was written with a single voice? Is the tone (e.g., educational, formal, friendly) consistent?
- **✅ Logical Flow:** Does each chapter naturally follow the one before it? Does the narrative make sense? For example, we shouldn't refer to a concept in Chapter 2 that isn't introduced until Chapter 4.
- **✅ Code and Command Correctness:** Double-check all code snippets and shell commands. Do they work as expected? Are they easy to copy and paste?
- **✅ Check for Typos and Grammar:** Use a spell checker and a grammar tool, but also read the content aloud. Reading aloud can help you catch awkward phrasing that software might miss.
- **✅ Verify Links:** Make sure all internal and external links work correctly. A broken link can be a frustrating experience for a reader.
- **✅ Image and Asset Check:** Ensure all images are displaying correctly and have appropriate alt text for accessibility.

## Deploying to GitHub Pages

Once you're happy with the content, it's time to deploy. GitHub Pages is a fantastic and free way to host your static Docusaurus site.

The process is surprisingly straightforward.

### Step 1: Push to a GitHub Repository

First, ensure your entire project is a GitHub repository and that you've pushed all your latest changes.

### Step 2: Configure `docusaurus.config.js`

Next, you need to tell Docusaurus where it will be deployed. Open your `docusaurus.config.js` file and add or update the following fields.

Replace `<Your-GitHub-Username>` and `<Your-Repo-Name>` with your actual GitHub username and repository name.

```javascript
// docusaurus.config.js

module.exports = {
  // ...
  title: 'My Awesome Book',
  url: 'https://<Your-GitHub-Username>.github.io', // Your GitHub pages URL
  baseUrl: '/<Your-Repo-Name>/', // The base URL for your project
  organizationName: '<Your-GitHub-Username>', // Your GitHub username
  projectName: '<Your-Repo-Name>', // Your repo name
  // ...
};
```

- `url`: The main URL for your GitHub Pages site.
- `baseUrl`: The path to your project on the server. For GitHub project pages, it's usually `/<repository-name>/`.
- `organizationName`: Your GitHub username.
- `projectName`: Your repository's name.

### Step 3: Run the Deployment Command

Docusaurus comes with a handy deployment command. Run the following in your terminal:

```bash
GIT_USER=<Your-GitHub-Username> npm run deploy
```

This command does two things:
1.  It builds a production-ready, optimized version of your site in the `build` directory.
2.  It pushes the contents of the `build` directory to a special branch in your repository called `gh-pages`.

### Step 4: Configure GitHub Pages Source

Finally, go to your repository's settings on GitHub. In the "Pages" section, ensure that the source for GitHub Pages is set to the `gh-pages` branch and the `/ (root)` folder.

![GitHub Pages Settings](https://raw.githubusercontent.com/facebook/docusaurus/main/website/static/img/docs/gh-pages-settings.png)

After a few minutes, your site will be live at the URL you configured in `docusaurus.config.js`!

## What's Next?

Congratulations! Your book is now live and available to the world.

But the journey doesn't end here. A good book, like good software, is a living document. You can continue to update it, add new chapters, and refine your existing content. With this deployment pipeline in place, every time you push a change to your main branch and run the deploy command, your site will be updated automatically.

In the final chapters, we'll discuss how to maintain and grow your project, gather feedback, and continue the cycle of spec-driven development.