# Self-Updating Portfolio with Gitfolio & GitHub Actions

This solution combines **Gitfolio** with **GitHub Actions** to create a fully automated system that keeps your portfolio fresh and up-to-date. With this setup, your GitHub acts as the source of your portfolio content, and GitHub Actions handles automatic updates and deployments to GitHub Pages.

## How This Works

This system uses **Gitfolio** and **GitHub Actions** to automate the creation, update, and deployment of your portfolio:

1. **Scheduled Runs**: GitHub Actions also runs every 10 minutes to ensure your portfolio stays updated, even if you forget to push changes.
2. **Effortless Deployment**: The workflow automatically deploys the updated portfolio to GitHub Pages, ensuring your portfolio is live and accessible 24/7.

## Features

- **Self-Updating**: With GitHub Actions running in the background, your portfolio gets updated every time you make a change to your repositories.
- **Automated Deployment**: The changes are automatically pushed to GitHub Pages, making your portfolio accessible online without manual intervention.
- **Scheduled Checks**: The system checks every 10 minutes for changes to ensure that your portfolio reflects your most recent work.
- **Easy Customization**: Customize your portfolio's layout and content by modifying Gitfolio's settings to match your personal style.

## Why Choose This Approach?

This setup eliminates the need for manual updates to your portfolio. Every time you push new code or make a change to your GitHub repository, the GitHub Actions workflow automatically handles the update and deployment of your portfolio to GitHub Pages. This ensures that your online presence is always up-to-date without any effort on your part!

## Customization and Extensibility

- **Custom Branch**: If you want to deploy from a branch other than `main` (e.g., `gh-pages`), simply adjust the workflow and GitHub Pages settings.
- **Customize Portfolio Layout**: Modify the `.gitfolio.yml` file to change the design, add sections, or filter repositories.
- **Frequent Updates**: Adjust the frequency of the GitHub Actions workflow if you need it to run more or less frequently.

## Credits

This solution relies on **Gitfolio** and **GitHub Actions** for automation. Special thanks to the developers of [Gitfolio](https://github.com/imfunniee/gitfolio) for creating such an amazing tool to easily build portfolios from GitHub repositories.

---

## Steps to Set Up Your Self-Updating Portfolio

### Step 1: Install Gitfolio

Gitfolio is a tool that generates a professional portfolio from your GitHub repositories. First, install Gitfolio globally on your system using npm:

```bash
npm install -g gitfolio
```

### Step 2: Build Your Portfolio

You can build your Gitfolio portfolio using two methods:

#### 1. **Using the UI**:
```bash
gitfolio ui
```
This command will launch an interactive UI that you can use to create and update your portfolio.

#### 2. **Using the Command Line**:
```bash
gitfolio build <username>
```
Replace `<username>` with your GitHub username. This command will build the portfolio and place the files in the `/dist` folder on your system.

Once the portfolio is built, you can run it locally:

```bash
gitfolio run -p [port]
```
You can specify a port number (default is port 3000) to run the portfolio locally. Navigate to `http://localhost:3000` in your browser to preview it.

For more details on building and customizing your portfolio, you can visit the [Gitfolio GitHub page](https://github.com/imfunniee/gitfolio).

### Step 3: Make Your Portfolio Live with GitHub Pages

Now that you have the portfolio running locally, it's time to make it live using GitHub Pages:

1. Go to **GitHub** and create a new repository named `username.github.io`, where `username` is your GitHub username.
2. Push the files from the `/dist` folder into this repository.

```bash
cd dist
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/username.github.io.git
git push -u origin main
```

3. Visit `https://username.github.io` in your browser to check if your site is up and running.

### Step 4: Enable GitHub Actions Permissions

To set up automated updates using GitHub Actions, enable the workflow permissions:

1. Go to your GitHub repository (where your portfolio is hosted).
2. Navigate to **Settings** → **Actions** → **General**.
3. Under **Workflow permissions**, select **Read and write permissions**.

### Step 5: Set Up GitHub Actions Workflow

1. Go to the **Actions** tab of your GitHub repository.
2. Click on **New Workflow** and create a new YAML file for GitHub Actions in the `.github/workflows` directory.

Here’s an example workflow that runs every 10 minutes to update the portfolio:

```yaml
name: Deploy Gitfolio Portfolio

on:
  push:
    branches:
      - main
  schedule:
    - cron: "*/10 * * * *"  # Run every 10 minutes

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Setup Node.js (for Gitfolio)
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install Gitfolio
        run: npm install -g gitfolio

      - name: Generate Gitfolio Portfolio
        run: gitfolio build <username>

      - name: Commit and Push Changes
        run: |
          git config user.name "GitHub Actions Bot"
          git config user.email "actions@github.com"
          git add -A
          git commit -m "Update Gitfolio portfolio" || echo "No changes to commit"
          git push origin main

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist  # Adjust if your output directory is different
```

### Step 6: Deploy to GitHub Pages

1. In your GitHub repository, go to **Settings** → **Pages**.
2. Under **Source**, select **gh-pages** (if your deployment branch is `gh-pages`).
3. Save your settings.

### Step 7: Final Check

After setting everything up, your portfolio will automatically update and deploy whenever there are changes. The GitHub Actions workflow will run every 10 minutes to ensure that your portfolio is up-to-date, based on the changes you’ve made to your repositories.

**Note**: The first push after setting up the workflow may take a few minutes. If necessary, try running it 2-3 times to get everything working smoothly.

### Step 8: Enjoy Your Live, Self-Updating Portfolio

Now your portfolio is live, and with every change you push to your repository, the GitHub Actions workflow will automatically update the portfolio. You can visit `https://username.github.io` anytime to see your updated portfolio.

---

## Additional Notes:

- **Customizing the Portfolio**: You can modify the `.gitfolio.yml` configuration file to filter repositories, change the layout, or add sections to your portfolio.
- **GitHub Pages Settings**: Ensure your GitHub Pages settings are correctly set to deploy from the right branch (`main` or `gh-pages`).
- **Frequent Updates**: The workflow is set to run every 10 minutes, but you can adjust this frequency by modifying the cron expression in the workflow.

---
