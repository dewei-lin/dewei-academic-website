# Academic Website Setup Guide (al-folio)

This guide walks you through creating an academic lab/personal website similar to sites like `thevazquezlab.com`, using the **al-folio** Jekyll template hosted free on **GitHub Pages**.

---

## Why al-folio?

It's the most popular academic-website template (~12k+ stars on GitHub) and is the engine behind hundreds of professor and lab pages. You get out of the box:

- Bio / About page with profile photo
- Publications page that auto-renders from a BibTeX file
- News feed
- Blog posts
- Optional CV, projects, teaching pages
- Dark mode, responsive design, MathJax for equations
- Free hosting on GitHub Pages

Source: https://github.com/alshedivat/al-folio
Live demo: https://alshedivat.github.io/al-folio/

---

## Prerequisites

You should have:

1. A GitHub account (free)
2. Git installed locally (`git --version` to check)
3. Ruby 3.0+ and Bundler if you want to preview locally — optional but recommended (`ruby -v`, `bundle -v`)

If you don't have Ruby, install via `brew install ruby` on macOS, then add it to your PATH.

---

## Step 1: Fork and clone the template

1. Go to https://github.com/alshedivat/al-folio
2. Click **"Use this template" → "Create a new repository"** (do NOT just fork — using the template gives you a clean repo with no commit history)
3. Name your repo. Two options:
   - **Personal site:** name it `<your-github-username>.github.io` — it will be served at `https://<username>.github.io/`
   - **Project/lab site:** name it anything (e.g., `lab-website`) — it will be served at `https://<username>.github.io/lab-website/`
4. Clone it locally into this folder:

```bash
cd ~/Documents/Claude/Projects/website
git clone https://github.com/<your-username>/<your-repo>.git .
```

---

## Step 2: Configure the site (`_config.yml`)

This is the single most important file. Open `_config.yml` and update:

```yaml
# Site metadata
title: Your Lab Name
first_name: Your
middle_name:
last_name: Name
email: you@university.edu
description: > # site description for SEO
  Your lab's tagline or one-line description.
url: https://<your-username>.github.io
baseurl: # leave empty for username.github.io repos; set to "/lab-website" for project repos

# Profile (top of About page)
keywords: machine learning, biology, neuroscience  # whatever fits

# Social links
github_username: your-username
twitter_username: your-handle
scholar_userid: <your Google Scholar ID>  # find in your Scholar profile URL
orcid_id: 0000-0000-0000-0000

# Disable things you don't need
news: true
blog_name: news      # rename "blog" to "news"
blog_nav_title: news
selected_papers: true  # show featured papers on home
social: true
```

There's a lot more in this file — read the comments carefully, they're well-documented.

---

## Step 3: Add your About page content

Edit `_pages/about.md`. The frontmatter at the top controls layout and the profile box (photo, address, social icons). The body below is your bio in Markdown.

```markdown
---
layout: about
title: about
permalink: /
subtitle: Affiliations. Address. Contacts.

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false
  address: >
    <p>Your office</p>
    <p>Your department</p>
    <p>Your university</p>

news: true
selected_papers: true
social: true
---

Write your bio here in Markdown. You can use **bold**, *italic*, [links](https://example.com), etc.
```

Replace `assets/img/prof_pic.jpg` with your own photo (same filename, or update the `image:` field).

---

## Step 4: Add publications

Open `_bibliography/papers.bib` and replace the sample entries with your own BibTeX. Each entry can include extra fields al-folio understands:

```bibtex
@article{yourkey2024title,
  title     = {Your Paper Title},
  author    = {Lastname, First and Coauthor, Second},
  journal   = {Journal Name},
  year      = {2024},
  volume    = {1},
  pages     = {1--10},
  doi       = {10.xxxx/yyyy},
  pdf       = {your-paper.pdf},     # file in assets/pdf/
  abstract  = {Short abstract...},
  selected  = {true},                # marks it as featured on the home page
  preview   = {paper-thumb.png}      # image in assets/img/publication_preview/
}
```

Drop PDFs in `assets/pdf/` and preview images in `assets/img/publication_preview/`.

---

## Step 5: Add news items

News posts are individual files in `_news/`. Filename format: `YYYY-MM-DD-short-title.md`.

```markdown
---
layout: post
date: 2026-05-01 12:00:00-0400
inline: true
related_posts: false
---

We just submitted a new paper to NeurIPS!
```

`inline: true` means it shows as a one-liner on the About page. Set `inline: false` and add a title to make it a full post.

---

## Step 6: Disable sections you don't need

You said you only want About, Publications, and News. Hide the rest by editing `_config.yml`:

```yaml
# In the navbar section, set enabled: false for unwanted pages
nav:
  - title: about
    permalink: /
  - title: publications
    permalink: /publications/
  # remove or comment out: blog, projects, teaching, cv
```

Or delete the corresponding files in `_pages/` (e.g., `projects.md`, `teaching.md`, `cv.md`).

---

## Step 7: Preview locally (optional but recommended)

```bash
cd ~/Documents/Claude/Projects/website
bundle install
bundle exec jekyll serve
```

Open http://localhost:4000 — your site will live-reload as you edit.

If you skip this, you can still preview by pushing to GitHub and looking at the GitHub Pages URL, but iteration is slower.

---

## Step 8: Deploy to GitHub Pages

al-folio ships with a GitHub Actions workflow (`.github/workflows/deploy.yml`) that builds the site automatically.

1. Push your changes:
   ```bash
   git add .
   git commit -m "Customize site"
   git push origin main
   ```
2. In your GitHub repo, go to **Settings → Pages**
3. Under **Build and deployment → Source**, select **GitHub Actions**
4. Wait ~2 minutes for the Actions workflow to finish (check the **Actions** tab)
5. Visit `https://<your-username>.github.io/` (or `/<repo-name>/` for project repos)

---

## Step 9: Custom domain (optional)

If you own a domain like `yourlab.com`:

1. Create a `CNAME` file in the repo root containing just `yourlab.com`
2. In your DNS provider, add an A record pointing to GitHub Pages IPs:
   - 185.199.108.153
   - 185.199.109.153
   - 185.199.110.153
   - 185.199.111.153
3. In **Settings → Pages**, enter your custom domain and check **Enforce HTTPS**

---

## Common customizations

| Want to change | Edit this |
|---|---|
| Site title, author, social links | `_config.yml` |
| Bio / about text | `_pages/about.md` |
| Profile photo | `assets/img/prof_pic.jpg` |
| Publications | `_bibliography/papers.bib` |
| News items | `_news/YYYY-MM-DD-title.md` |
| Theme colors | `_sass/_themes.scss` (CSS variables) |
| Navbar items | `nav:` block in `_config.yml` |
| Favicon | `assets/img/favicon.ico` |
| Footer text | `_includes/footer.html` |

---

## Troubleshooting

- **Site not building?** Check the **Actions** tab on GitHub for the error log.
- **Local preview fails on `bundle install`?** Make sure your Ruby is 3.0+ (`ruby -v`).
- **Publications not rendering?** Validate your `.bib` file — one bad entry breaks the whole list.
- **404 on GitHub Pages?** For project repos, make sure `baseurl` in `_config.yml` matches `/your-repo-name`.

---

## Alternative: academicpages

If al-folio feels too opinionated, try [academicpages](https://github.com/academicpages/academicpages.github.io) — same fork-and-customize flow, slightly different aesthetic, more emphasis on CV-style pages. Setup is essentially identical.
