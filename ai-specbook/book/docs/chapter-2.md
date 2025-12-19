# Chapter 2: Docusaurus Setup and Markdown Basics
    6 
    7 With our vision established, it's time to lay the foundation for our book. This chapter will guide you through 
      setting up a Docusaurus project from scratch and mastering the Markdown basics you'll need to write your conten      This is a hands-on chapter, so get ready to run some commands!
    8 
    9 ## Setting Up Your Docusaurus Site
   10 
   11 Docusaurus is a static site generator that turns your Markdown files into a beautiful, easy-to-navigate website      Let's get it running.
   12 
   13 ### Prerequisites
   14 
   15 Before you start, make sure you have **Node.js** installed. Docusaurus requires a recent version (e.g., 18.0 or      later). You can download it from the official [Node.js website](https://nodejs.org/).
   16 
   17 You can check if you have it installed by running this command in your terminal:
  node -v
   1 
   2 ### Step 1: Create a New Docusaurus Site
   3 
   4 Open your terminal, navigate to where you want to create your project, and run the following command:
  npx create-docusaurus@latest ai-specbook classic

    1 
    2 Let's break that down:
    3 - `npx create-docusaurus@latest`: This is the command that always fetches the latest version of the Docusaurus 
      installer.
    4 - `ai-specbook`: This will be the name of the directory created for your project. You can change this to whatev      you like.
    5 - `classic`: This tells the installer to use the "classic" template, which comes pre-configured with a blog,   
      documentation, and pages, which is perfect for our needs.
    6 
    7 The installer will ask you a few questions about whether you want to use TypeScript. For simplicity, we'll choo      **No** for now.
    8 
    9 ### Step 2: Start the Development Server
   10 
   11 Once the installation is complete, navigate into your new project directory:
  cd ai-specbook
   1 
   2 Now, start the local development server:
  npm run start
   1 
   2 This command will build your site and start a server. After a moment, your terminal should show something like  
     this:
  [SUCCESS] Docusaurus site is running at http://localhost:3000/
    1 
    2 Open your web browser and go to `http://localhost:3000`. You should see the default Docusaurus landing page.   
      Congratulations, you have a running Docusaurus site! The server will automatically reload as you make changes t      the files.
    3 
    4 ### Step 3: Understanding the Project Structure
    5 
    6 Your `ai-specbook` directory contains several files and folders. Here are the most important ones for a beginne    7 
    8 - **`/docs/`**: This is where we'll write our book! All the `.md` or `.mdx` files in here become documentation 
      pages.
    9 - **`/blog/`**: This is for any blog posts you might want to write.
   10 - **`/src/pages/`**: For creating standalone pages, like a landing page or a contact page.
   11 - **`docusaurus.config.js`**: This is the heart of your site's configuration. You'll edit this file to change t      title, navigation bar, footer, and more.
   12 
   13 ## Markdown Basics for Docusaurus
   14 
   15 Markdown is a lightweight markup language with plain-text formatting syntax. It's designed to be easy to read a      easy to write.
   16 
   17 Here are the essential elements you'll use constantly.
   18 
   19 ### Headings
   20 
   21 Use hash symbols (`#`) to create headings. The number of hashes determines the heading level.
  Level 1 Heading (Your Page Title)
  Level 2 Heading (A Major Section)
  Level 3 Heading (A Subsection)

    1 
    2 ### Text Formatting
    3 
    4 - **Bold**: `**This text will be bold**`
    5 - *Italic*: `*This text will be italic*`
    6 - `Code`: `` `This will be formatted as inline code` ``
    7 
    8 ### Lists
    9 
   10 **Unordered List:**
   - First item
   - Second item
     - A nested item
   1 
   2 **Ordered List:**
   1. First step
   2. Second step
   3. Third step
   1 
   2 ### Code Blocks
   3 
   4 For showing multi-line code snippets, use triple backticks (```` ``` ````). You can also specify the programming     language for syntax highlighting.
   5 
   1 ```python
   2 def hello_world():
   3   print("Hello from a code block!")
   4 ```

  Links and Images

   - Link: [Visit the Docusaurus website](https://docusaurus.io/)
   - Image: ![Docusaurus Logo](/img/logo.svg)

  Docusaurus Admonitions

  Docusaurus enhances standard Markdown with "admonitions," which are special blocks for highlighting information.
  They are incredibly useful!

    1 :::note
    2 
    3 This is a note. It's great for information that's important but doesn't need to block the user.
    4 
    5 :::
    6 
    7 :::tip
    8 
    9 Use tips to offer helpful advice or a shortcut.
   10 
   11 :::
   12 
   13 :::info
   14 
   15 Info blocks are for neutral, informative content.
   16 
   17 :::
   18 
   19 :::caution
   20 
   21 Use caution for things that might have unexpected results if not handled carefully.
   22 
   23 :::
   24 
   25 :::danger
   26 
   27 Danger blocks should be used for critical warnings, like actions that could lead to data loss.
   28 
   29 :::

  What's Next?

  You now have a running Docusaurus site and the fundamental Markdown skills to start writing. In the next chapter,  
  we'll put these skills to use by writing our very first technical specification for our GeminiModel client,        
  combining narrative Markdown with structured YAML.