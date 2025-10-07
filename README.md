# GitHub Pages Deployment Guide

This repository provides a copy-pasteable template for deploying a static page to GitHub Pages while serving it from a Squarespace-managed domain. The guide walks through configuring DNS, GitHub Pages, and optional automations so you can publish under a subdomain such as `dojo.randroid.dev` while Squarespace continues hosting the apex domain.

## 1. Create the repository

1. Create a new public repository in your GitHub account (for example, `dojo-site`).
2. Add the provided `index.html` file to the root of the repository and commit it to the default branch (usually `main`).

## 2. Enable GitHub Pages

1. Navigate to **Settings → Pages** in the repository.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select the `main` branch and the `/ (root)` folder, then click **Save**.
4. GitHub Pages will publish the site at `https://<your-username>.github.io/<repo-name>/` as a temporary URL while DNS is configured.

## 3. Point a Squarespace subdomain to GitHub Pages

1. Decide on a subdomain (e.g., `dojo.randroid.dev`).
2. In Squarespace, go to **Settings → Domains → randroid.dev → DNS**.
3. Add a CNAME record:
   - **Type:** `CNAME`
   - **Host / Name:** the subdomain label (e.g., `dojo`)
   - **Value / Target:** `<your-username>.github.io`
   - **TTL:** `1 hour` is fine
4. Squarespace automatically appends `.randroid.dev`, so only enter the subdomain label (e.g., `dojo`).

## 4. Add the custom domain in GitHub Pages

1. Return to **Settings → Pages** in the repository.
2. Enter the full subdomain (e.g., `dojo.randroid.dev`) in the **Custom domain** field and click **Save**.
3. Wait for the domain to verify and for GitHub to provision the SSL certificate. Once validated, GitHub will create a `CNAME` file in the repository. Keep this file committed.

## 5. Integrate with Squarespace navigation

Choose one of the following to link the GitHub Pages site from Squarespace:

- **Add a navigation link:** In **Pages**, add a new Link, set the title (e.g., `Dojo`), and point it to `https://dojo.randroid.dev`.
- **Create a redirect:** Go to **Settings → Advanced → URL Mappings** and add `/dojo -> https://dojo.randroid.dev 301` to redirect `https://randroid.dev/dojo` to the GitHub Pages site.

## 6. Optional: Automate deployments with GitHub Actions

Add the workflow file at `.github/workflows/pages.yml` to build and deploy the site automatically on every push to `main`. The workflow included in this repository is ready to use for simple static sites. If you have a build step (e.g., bundling assets), insert the necessary commands before the "Upload artifact" step.

## Verification tips

- Run `dig CNAME dojo.randroid.dev` (replace with your subdomain) to confirm that the DNS points to `<your-username>.github.io`.
- Visit `https://dojo.randroid.dev` (or your chosen subdomain) to verify the page loads.
- SSL provisioning can take a little time after DNS updates; check back if you see a certificate warning initially.

Feel free to replace the placeholder names with your actual GitHub username, repository name, and desired subdomain.
