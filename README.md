# Visitor Badge Installation Guide 🚀

![Visits](https://img.shields.io/badge/Visits-16845-blue)

Welcome to the `visitor-badge` repository by [Arman Kianian](https://github.com/Armanx200)! This guide will help you set up and use the visitor badge to track visits to your GitHub repository. Let's get started! 🎉

## Prerequisites 📋

Before you begin, make sure you have:

- A GitHub account
- A repository where you want to add the visitor badge
- Basic knowledge of Git and GitHub

## Step-by-Step Installation 🛠️

### 1. Fork the Repository 🍴

First, fork the [visitor-badge repository](https://github.com/Armanx200/visitor-badge) to your GitHub account.

### 2. Clone the Repository 📂

Next, clone the forked repository to your local machine:

```bash
git clone https://github.com/<your-username>/visitor-badge.git
cd visitor-badge
```

### 3. Set Up the Visit Counter 🚥

Ensure you have a `visits.txt` file in the root of your repository to track the visit count. If it doesn't exist, create it:

```bash
echo "0" > visits.txt
```

### 4. Add the Badge to Your README.md 📝

Update your `README.md` file to include the visit badge. Add the following line where you want the badge to appear:

```markdown
![Visits](https://img.shields.io/badge/Visits-16845-blue)
```

### 5. Set Up GitHub Actions ⚙️

Create a workflow file to automate visit count updates. Create a directory `.github/workflows` in your repository if it doesn't already exist, then create a file named `update-visits.yml`:

```yaml
name: Update Visits

on:
  push:
    branches:
      - main
  schedule:
    - cron: '*/15 * * * *' # Runs every 15 minutes

jobs:
  update-visits:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Increment visit counter
        id: increment
        run: |
          if [ ! -f visits.txt ]; then echo "0" > visits.txt; fi
          visits=$(cat visits.txt)
          visits=$((visits + 1))
          echo $visits > visits.txt
          echo "::set-output name=visits::$visits"

      - name: Update README.md
        run: |
          visits=${{ steps.increment.outputs.visits }}
          badge="![Visits](https://img.shields.io/badge/Visits-16845-blue)"
          sed -i 's|!\[Visits\](https://img.shields.io/badge/Visits-.*-blue)|'"$badge"'|' README.md

      - name: Commit changes
        run: |
          git config --global user.name 'github-actions'
          git config --global user.email 'github-actions@github.com'
          git add visits.txt README.md
          git commit -m 'Update visits count'
          git push
```

### 6. Commit and Push Your Changes 🚀

Commit and push the changes to your GitHub repository:

```bash
git add visits.txt README.md .github/workflows/update-visits.yml
git commit -m "Set up visit tracking workflow"
git push origin main
```

### 7. Monitor Your Visits 📊

Once everything is set up, your visit count will be updated automatically every 15 minutes. You can view the visit badge in your `README.md`:

![Visits](https://img.shields.io/badge/Visits-16845-blue)

## Congratulations! 🎉

You have successfully set up the visitor badge in your repository! Now you can track how many people visit your GitHub project. Happy coding! 💻
