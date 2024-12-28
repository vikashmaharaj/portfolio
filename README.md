## Self-Updating Portfolio with Gitfolio & GitHub Actions

### Overview

This system combines **Gitfolio** with **GitHub Actions** to automate updates and deployment of your Gitfolio portfolio to **GitHub Pages**. Gitfolio automatically builds a portfolio from your GitHub repositories, and GitHub Actions updates and deploys this portfolio whenever changes are made.

### How This Works

1. **Automatic Updates**: Every time you push changes to your GitHub repositories, GitHub Actions automatically triggers the workflow to update your Gitfolio portfolio.
2. **Scheduled Runs**: GitHub Actions also runs every 10 minutes to ensure that your portfolio is kept fresh, even if you forget to push changes.
3. **Effortless Deployment**: The workflow ensures that the portfolio is automatically deployed to **GitHub Pages** so that it’s live 24/7.

### Features

- **Self-Updating**: Every push to your repositories automatically updates your portfolio through GitHub Actions.
- **Automated Deployment**: No manual intervention is required. Changes are pushed to **GitHub Pages** automatically.
- **Scheduled Checks**: GitHub Actions checks every 10 minutes for changes, ensuring your portfolio stays up to date.
- **Customizable**: You can modify Gitfolio settings to personalize the layout, content, and sections of your portfolio.

### Why Choose This Approach?

This setup allows your portfolio to stay up to date automatically, without needing any manual intervention. Whenever you push code to GitHub, the GitHub Actions workflow will take care of updating the portfolio and deploying it to GitHub Pages.

---

### Customization and Extensibility

1. **Custom Branch**: If you wish to deploy from a branch other than `main` (for example, `gh-pages`), you can easily change the workflow and GitHub Pages settings accordingly.
2. **Modify Portfolio Layout**: The `.gitfolio.yml` file lets you modify your portfolio's layout, filter repositories, or add custom sections to better represent your work.
3. **Adjust Workflow Frequency**: You can modify the frequency of the workflow. If you need the portfolio to update more or less frequently, you can adjust the cron expression in the GitHub Actions workflow file.

### Credits

This approach leverages **Gitfolio** for generating a portfolio and **GitHub Actions** for automating updates and deployments. Special thanks to the creator of [Gitfolio](https://github.com/imfunniee/gitfolio) for providing a simple solution for building GitHub-based portfolios.


---

### Steps to Set Up Your Self-Updating Portfolio

#### Step 1: Install Gitfolio

First, install Gitfolio globally using npm:
```bash
npm install -g gitfolio
```

#### Step 2: Build Your Portfolio

You can build your Gitfolio portfolio using two methods:

##### 1. **Using the UI**:
```bash
gitfolio ui
```
This command opens an interactive UI where you can create and customize your portfolio.

##### 2. **Using Command Line**:
```bash
gitfolio build <username>
```
Replace `<username>` with your GitHub username. This will generate the portfolio files in a folder (by default, it will be `/dist`).

To preview the portfolio locally:
```bash
gitfolio run -p [port]
```
Replace `[port]` with the desired port (default is 3000). Then navigate to `http://localhost:3000` in your browser.

#### Step 3: Deploy to GitHub Pages

1. Create a new GitHub repository for your portfolio, typically named `username.github.io`, where `username` is your GitHub username.
2. Push the contents of the `/dist` folder into this repository:
```bash
cd dist
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/username.github.io.git
git push -u origin main
```

3. Your portfolio will now be live at `https://username.github.io`.

#### Step 4: Enable GitHub Actions

1. Go to **Settings** → **Actions** → **General** for your GitHub repository.
2. Under **Workflow permissions**, select **Read and write permissions**.

#### Step 5: Create GitHub Actions Workflow

Create a `.github/workflows` directory in your repository and add a `deploy.yml` file with the following content:

```yaml
name: Build and Deploy Gitfolio

on:
  push:
    branches:
      - main  # Trigger on push to the main branch

  schedule:
    - cron: "*/10 * * * *"  # Run every 10 minutes


jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout repository
        uses: actions/checkout@v3

      # Step 2: Pull the latest changes from the repository
      - name: Pull latest changes
        run: |
          git fetch origin
          git reset --hard origin/main  # Replace 'main' with your default branch

      # Step 3: Install dependencies
      - name: Install dependencies
        run: npm install gitfolio -g

      # Step 4: Generate Gitfolio Portfolio
      - name: Generate Gitfolio Portfolio
        run: gitfolio update

      # Step 5: Commit and push changes
      - name: Commit and Push Changes
        run: |
          git config user.name "GitHub Actions Bot"
          git config user.email "actions@github.com"
          git add .
          git commit -m "Update Gitfolio portfolio" || echo "No changes to commit"
          git push origin main


      # Step 6: Deploy to GitHub Pages
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist  # Replace with the directory containing generated files
```

#### Step 6: Configure GitHub Pages

1. Go to **Settings** → **Pages** in your GitHub repository.
2. Set the source branch to `gh-pages` (or `main` if you prefer).
3. Save the settings.

#### Step 7: Enjoy Your Live Portfolio

Once you’ve completed the above steps, your portfolio will update automatically whenever you push new code or make changes to your repositories. You can visit your live portfolio at `https://username.github.io`.

---

With this setup, you’ll have a **self-updating portfolio** that automatically reflects your most recent work. The GitHub Actions workflow will ensure that your portfolio is continuously updated without any additional effort on your part!
