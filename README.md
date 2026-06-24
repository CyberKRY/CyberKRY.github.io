# 🛡️ Cybersecurity Portfolio — Jekyll + GitHub Pages

A clean, dark portfolio site for security researchers and penetration testers.

---

## 🚀 Quick Start — Deploy to GitHub Pages

### 1. Create the repository

Go to [github.com/new](https://github.com/new) and create a repository named exactly:

```
YOUR_GITHUB_USERNAME.github.io
```

### 2. Upload these files

```bash
git init
git add .
git commit -m "Initial portfolio"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/YOUR_GITHUB_USERNAME.github.io.git
git push -u origin main
```

### 3. Enable GitHub Pages

- Go to your repo → **Settings** → **Pages**
- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`
- Save → your site will be live at `https://YOUR_GITHUB_USERNAME.github.io` in ~2 minutes

---

## ✏️ Personalizing the Site

### Things to replace (search for these strings):

| Placeholder        | Replace with                         |
|--------------------|--------------------------------------|
| `YOUR_NAME`        | Your real name or handle             |
| `YOUR_GITHUB`      | Your GitHub username                 |
| `YOUR_LINKEDIN`    | Your LinkedIn username               |
| `YOUR_TWITTER`     | Your Twitter/X handle                |
| `your@email.com`   | Your contact email                   |
| `YOUR_YEAR`        | Year you got the cert                |
| `XX+`              | Your actual stats                    |

---

## 📝 Adding a New Write-up

Create a file in `_posts/` with the naming format:

```
_posts/YYYY-MM-DD-platform-machinename.md
```

Example: `_posts/2025-03-10-htb-blue.md`

**Front matter template:**

```yaml
---
layout: post
title: "HackTheBox — Blue [Write-up]"
date: 2025-03-10
platform: htb          # htb | thm | portswigger | other
difficulty: Easy       # Easy | Medium | Hard
category: write-up
tags: [windows, smb, eternalblue]
description: "Short description shown in the write-ups list."
---

Your write-up content in Markdown here...
```

Then just:

```bash
git add .
git commit -m "Write-up: HTB Blue"
git push
```

Site updates automatically in ~1-2 minutes.

---

## 📁 Adding a Project

Create a file in `_projects/`:

```
_projects/htb-blue.md
```

```yaml
---
layout: project
title: "HackTheBox — Blue"
date: 2025-03-10
platform: htb          # htb | thm | portswigger | other
difficulty: Medium
featured: true         # shows on homepage
tags: [windows, eternalblue, ms17-010]
description: "One-line description for the project card."
---

Longer project description in Markdown...
```

---

## 🏷️ Platform Colors

| Value        | Color  | Platform       |
|--------------|--------|----------------|
| `htb`        | 🟢 Green  | HackTheBox     |
| `thm`        | 🔴 Red    | TryHackMe      |
| `portswigger`| 🟠 Orange | PortSwigger    |
| `other`      | 🔵 Blue   | Other / CTF    |

---

## 📁 File Structure

```
portfolio/
├── _config.yml          ← Site settings & your info
├── index.html           ← Homepage
├── _layouts/
│   ├── default.html     ← Nav + footer wrapper
│   ├── post.html        ← Write-up page layout
│   └── project.html     ← Project page layout
├── _posts/              ← Write-ups (YYYY-MM-DD-name.md)
├── _projects/           ← Project pages
└── assets/
    └── css/
        └── main.css     ← All styles
```
