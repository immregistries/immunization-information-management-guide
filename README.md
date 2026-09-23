# Immunization Information Management Guide

This repository contains the Markdown source for a MkDocs-based documentation site for the **Immunization Information Management Guide**.

## Local preview

1. Create and activate a Python virtual environment.
2. Install dependencies:
   `pip install -r requirements.txt`
3. Start the local preview server:
   `mkdocs serve`
4. Open `http://127.0.0.1:8000/`

## Local build

Run:

`mkdocs build --strict`

The generated site is written to `site/`.

## Publishing

GitHub Actions builds the site on pull requests and deploys it from `main` with GitHub Pages.

### One-time GitHub Pages setup

In the repository on GitHub:

1. Go to **Settings**.
2. Open **Pages**.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.

## Notes

- The guide is independent working documentation and is not presented as endorsed by AIRA, CDC, ISO, or HL7.
- A content license has **not** been selected in this repository; that is an editorial decision for the repository owner.
