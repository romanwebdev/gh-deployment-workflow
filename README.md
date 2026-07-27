# GitHub Pages Deployment with GitHub Actions

## Technologies

- Git
- GitHub
- GitHub Actions
- GitHub Pages
- YAML
- HTML

---

## Goal

The goal of this project is to automate the deployment of a static website to GitHub Pages using GitHub Actions.

The workflow should:

- Deploy the website automatically after every push to the `main` branch.
- Trigger only when the `index.html` file changes.
- Publish the latest version of the website to GitHub Pages.

---

## Steps

### 1. Create the Repository

Create a GitHub repository containing:

- `index.html`
- `README.md`

Example `index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>GitHub Pages</title>
  </head>
  <body>
    <h1>Hello, GitHub Actions!</h1>
  </body>
</html>
```

---

### 2. Create the GitHub Actions Workflow

Create the workflow file:

```
.github/workflows/deploy.yml
```

Configure the workflow to:

- Run on every push to the `main` branch.
- Trigger only when `index.html` changes.
- Deploy the repository contents to GitHub Pages.

Workflow trigger:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - index.html
```

Grant only the permissions required for deployment:

```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

These permissions allow the workflow to:

- Read the repository contents.
- Publish files to GitHub Pages.
- Authenticate securely using GitHub's OIDC token.

Prevent simultaneous deployments by configuring concurrency:

```yaml
concurrency:
  group: pages
  cancel-in-progress: true
```

If multiple commits are pushed in a short period of time, any older deployment is cancelled and only the latest version is published.

The deployment job performs the following steps:

1. Checkout the repository.
2. Configure GitHub Pages.
3. Upload the website as a Pages artifact.
4. Deploy the artifact to GitHub Pages.

Main deployment steps:

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v7

  - name: Setup Pages
    uses: actions/configure-pages@v6

  - name: Upload artifact
    uses: actions/upload-pages-artifact@v5
    with:
      path: .

  - name: Deploy to GitHub Pages
    id: deployment
    uses: actions/deploy-pages@v5
```

---

### 3. Enable GitHub Pages

In the repository:

```
Settings
    → Pages
        → Build and deployment
            → Source: GitHub Actions
```

GitHub will automatically publish the website after a successful workflow run.

---

### 4. Verify the Deployment

After pushing changes to `index.html`:

- Open the **Actions** tab.
- Verify that the workflow completed successfully.
- Open the GitHub Pages URL: [https://romanwebdev.github.io/gh-deployment-workflow](https://romanwebdev.github.io/gh-deployment-workflow)

The updated website should be available.

---

## Outcome

A fully automated deployment pipeline was configured for a static website using GitHub Actions and GitHub Pages. The workflow filters events by branch and file path, securely deploys the website, and publishes the latest version automatically after qualifying changes.

---

## Link

[roadmap.sh](https://roadmap.sh/projects/github-actions-deployment-workflow)
