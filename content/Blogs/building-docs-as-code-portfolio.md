---
title: "Building a GitHub Portfolio: A Step-by-Step Guide for Technical Writers"
date: 2026-01-10
summary: "Why traditional PDFs don't cut it anymore, and how to engineer a high-performance portfolio using Hugo, PaperMod, and Markdown."
showToc: true
TocOpen: false
---

If you are a technical writer looking for your next role, I have a bittersweet truth for you: **sending a static PDF resume or a generic Google Docs link is no longer enough.** 

Engineering managers and documentation directors don't just want to know if you can write a clean sentence. They want to know if you can survive in a modern development environment. They want to know if you speak **Docs-as-Code**.

When I decided to revamp my portfolio, I chose to treat it exactly like an enterprise software project. I built it using **Hugo** (a fast static site generator), the minimalist **PaperMod** theme, and pure **Markdown**. 

In this guide, I’ll walk you through exactly how to build your own high-performance portfolio from scratch, sharing the hidden configuration traps I encountered and how to structure your projects to turn heads.

---

## The Tech Stack: Why Hugo?

There are plenty of no-code site builders out there (WordPress, Wix, Squarespace), but they send the wrong signal. Using a static site generator proves three things instantly:
1. You are comfortable using the **terminal/command line**.
2. You understand **Markdown** syntax and file hierarchy.
3. You know how to manage web assets and local compilation frameworks.

Hugo is incredibly fast, generates lightweight HTML, and integrates flawlessly with free hosting platforms like GitHub Pages.

---

## Step 1: Initialize Your Project

First, ensure you have Hugo installed on your machine (via Homebrew for Mac or Chocolatey for Windows). Open your terminal and run the following command to create your new site structure:

```bash
hugo new site technical-writer-portfolio
cd technical-writer-portfolio
```

This creates a clean directory tree containing vital folders like `/content` (where your writing goes) and `/static` (where your images and downloadable assets live).

---

## Step 2: Install and Hook Up the Theme

For a technical writing site, minimalism is your best friend. The **PaperMod** theme cuts out heavy JavaScript animations and visual clutter, putting your words, typography, and information architecture front and center.

Clone the theme directly into your project's `/themes` directory using Git:

```bash
git clone [https://github.com/adityatelange/hugo-PaperMod.git](https://github.com/adityatelange/hugo-PaperMod.git) themes/hugo-papermod
```

> ⚠️ **The Configuration Trap:** Folder names are case-sensitive! If your cloned theme folder is named `hugo-PaperMod` (with a capital P) but your configuration file looks for `hugo-papermod`, your site will crash or render as a blank white text list. Ensure they match perfectly.

---

## Step 3: Engineer Your `hugo.toml`

The `hugo.toml` file at the root of your directory is the engine room of your portfolio. Instead of just setting up a basic landing page, you want a dynamic setup with direct navigation buttons, social links, and an active homepage.

Here is the blueprint structure I engineered for my site, featuring **four main action buttons** to guide recruiters immediately to the high-value areas:

```toml
baseURL = '[https://yourusername.github.io/](https://yourusername.github.io/)'
languageCode = 'en-us'
title = 'Your Name Portfolio'
theme = 'hugo-papermod'
enableEmoji = true

[menu]
  [[menu.main]]
    name = "Home"
    url = "/"
    weight = 1
  [[menu.main]]
    name = "Projects"
    url = "/projects/"
    weight = 2

[params]
  title = "Your Name"
  defaultTheme = "auto" # Toggles light/dark mode automatically based on OS

  [params.profileMode]
    enabled = true
    title = "Your Name"
    subtitle = "Senior Technical Writer specializing in Docs-as-Code architecture, API design, and enterprise guidelines."
    imageUrl = "images/profile.jpg" # Circular avatar picture
    
    # Quad Action Control Dashboard
    [[params.profileMode.buttons]]
      name = "📄 View Experience"
      url = "/resume/"
    [[params.profileMode.buttons]]
      name = "👤 About Me"
      url = "/about/"
    [[params.profileMode.buttons]]
      name = "🚀 Projects"
      url = "/projects/"
    [[params.profileMode.buttons]]
      name = "✍️ Blogs"
      url = "/blog/"
```

---

## Step 4: Master the File Hierarchy

Hugo differentiates between independent standalone pages (like an "About Me" or "Resume" page) and structural lists of content (like your projects or blogs). 

To ensure your layout displays correctly, construct your `/content` directory like this:

* `content/`
  * `about.md` *(Standalone About page)*
  * `resume.md` *(Standalone Resume page)*
  * `projects/` *(A section folder)*
    * `_index.md` *(Crucial! Tells Hugo to treat this folder as a dashboard catalog)*
    * `case-study-one.md`
    * `case-study-two.md`

Inside `content/projects/_index.md`, include this front-matter metadata to force PaperMod to render individual project summaries as beautiful scannable cards:

```markdown
---
title: "Projects & Documentation Case Studies"
description: "Real-world breakdowns of technical frameworks, guidelines, and product suites."
layout: "list"
---
```

---

## Step 5: Write Case Studies, Not Asset Links

Here is where many technical writing portfolios fail: they just present a bulleted list of URLs to external document files. Links break, company domains change, and public access gets revoked.

Instead, frame your work as a **Documentation Case Study** using the **CAR Formula**:
* **Context:** What company were you at, who was the audience, and what was the technological ecosystem?
* **Action:** What strategies did you implement? Did you apply standard style guides (like MSTP)? Did you map requirements (BRD/FRD/SRD) directly inside Agile sprints via Azure DevOps or Jira?
* **Result:** What was the actual impact? Did deployment errors drop? Did developer onboarding speed up?

### Pro-Tip: Hosting Static Assets Directly
To ensure your portfolio stands the test of time, store your work directly inside your local source tree. 
* Drop your interface screenshots into `static/images/`
* Put your local PDF files or whitepapers into `static/downloads/`

You can then link to them directly inside your markdown files using pathing formats like `[📥 Download Whitepaper (PDF)](/downloads/your-file.pdf)`. When a recruiter clicks the link, the browser will seamlessly open the file in a new tab without sending them away from your website.

---

## Step 6: Forcing Hero Covers and the Table of Contents

By default, PaperMod can hide your post images or compress your side navigation menus. To maximize the readability of a long-form case study, adjust your Markdown front-matter fields to explicitly dictate the layout rules:

```markdown
---
title: "Enterprise Product Documentation Suite"
date: 2026-06-16
summary: "Architecting end-to-end user journeys and technical frameworks."

# Force Display of Hero Layouts
cover:
    image: "images/project-hero.png"
    alt: "Product Interface Mockup"
    caption: "The fully documented software architecture ecosystem"
    relative: false
    hidden: false         # Displays on the main portfolio list grid
    hiddenInSingle: false # Displays at the top of the article page

# Make Documentation Skimmable
showToc: true
TocOpen: true # Forces the Table of Contents to stay open by default
---
```

*Note: If your Table of Contents is skipping sections, check your heading weights. PaperMod builds its index tables using standard `##` (Heading 2) parameters. Ensure your core text milestones follow a clean hierarchical path!*

---

## Launching to the Cloud

Once your local server (`hugo server`) is building smoothly without warnings, it's time to publish. You can initialize a Git repository at your root folder, push the source files to a private or public **GitHub** repository, and use **GitHub Actions** to automatically build and host your final distribution files on **GitHub Pages** for free.

By building your portfolio this way, you aren't just telling hiring managers you understand Docs-as-Code. The moment they load your website, you've already proven it.

***

*Have questions? connect with me via my homepage links!*
---