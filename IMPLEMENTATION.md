 Implementation Journal (IMPLEMENTATION.md)
md
Copy
Edit
# 📖 Implementation Journal: Docusaurus Setup, Deployment, and Containerization  

> **Author**: Sagar Raghuvanshi  
> **Date**: February 16, 2025  
> **Project**: Docusaurus Documentation Site  

## 🔍 Overview  

This document outlines the **step-by-step implementation** of setting up, deploying, and containerizing a **Docusaurus documentation site**. The goal is to:  

- Set up a **Docusaurus site** locally  
- Deploy the site using **GitHub Pages**  
- Implement **automatic deployment** via **GitHub Actions**  
- Containerize the site using **Podman**  

---

## 📌 1. Docusaurus Setup  

### 🔹 Step 1: Create a New Docusaurus Project  

To create a **Docusaurus** project, I ran the following command:  

```sh
npx create-docusaurus@latest my-task classic
cd my-task
npm install
This initialized a Docusaurus site inside the my-task folder.

🔹 Step 2: Run the Development Server
To test if the site runs correctly, I started the local server:

sh
Copy
Edit
npm run start
The site was accessible at:
📍 http://localhost:3000

📌 2. Setting Up GitHub Repository
To enable version control, I initialized a Git repository and pushed the project to GitHub:

sh
Copy
Edit
cd my-task
git init
git remote add origin https://github.com/SagarRaghuvanshi31/my-task.git
git branch -M main
git add .
git commit -m "Initial commit: Docusaurus setup"
git push -u origin main
📌 3. Configuring GitHub Pages Deployment
🔹 Step 1: Modify docusaurus.config.js
I updated the GitHub Pages configuration inside docusaurus.config.js:

js
Copy
Edit
const config = {
  title: 'Meri Docs Site',
  url: 'https://SagarRaghuvanshi31.github.io',
  baseUrl: '/my-task/',
  organizationName: 'SagarRaghuvanshi31',
  projectName: 'my-task',
  deploymentBranch: 'gh-pages',
};

export default config;
🔹 Step 2: Push the Changes
sh
Copy
Edit
git add docusaurus.config.js
git commit -m "Update Docusaurus config for GitHub Pages"
git push origin main
📌 4. Automating Deployment with GitHub Actions
🔹 Step 1: Create GitHub Actions Workflow
I created a GitHub Actions workflow to deploy the site automatically.

sh
Copy
Edit
mkdir -p .github/workflows
nano .github/workflows/deploy.yml
Inside deploy.yml, I added:

yaml
Copy
Edit
name: Deploy Docusaurus

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Build site
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
🔹 Step 2: Push Workflow to GitHub
sh
Copy
Edit
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions for auto deployment"
git push origin main
🔹 Step 3: Enable GitHub Pages
Open GitHub Repository → Settings → Pages
Set Branch to gh-pages
Click Save
Now, the site was automatically deployed at:
🌍 https://SagarRaghuvanshi31.github.io/my-task/

📌 5. Setting Up Auto-Refresh for Local Development
To automatically update the local site when new changes were pulled, I created an auto-refresh script:

sh
Copy
Edit
nano ~/my-task/auto_refresh.sh
Added the following Bash script:

sh
Copy
Edit
#!/bin/bash
while true; do
    echo "Checking for updates..."
    git -C ~/my-task pull origin main | grep -q "Already up to date" || {
        echo "Restarting Docusaurus..."
        pkill -f "npm run start"
        cd ~/my-task && nohup npm run start > ~/my-task/docusaurus.log 2>&1 &
    }
    sleep 1
done
Made it executable:

sh
Copy
Edit
chmod +x ~/my-task/auto_refresh.sh
nohup ~/my-task/auto_refresh.sh > ~/my-task/refresh.log 2>&1 &
📌 6. Containerizing Docusaurus with Podman
🔹 Step 1: Create a Dockerfile
sh
Copy
Edit
nano Dockerfile
Inside Dockerfile:

dockerfile
Copy
Edit
FROM node:18-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["npm", "start", "--", "--host", "0.0.0.0"]
🔹 Step 2: Build the Podman Image
sh
Copy
Edit
podman build -t docusaurus-image .
🔹 Step 3: Run the Container
sh
Copy
Edit
podman run -d -p 3001:3000 --name docusaurus-container docusaurus-image
Verify if the container is running:

sh
Copy
Edit
podman ps
Test the running container:

sh
Copy
Edit
curl http://localhost:3001
📌 7. Pushing the Image to Docker Hub
🔹 Step 1: Tag the Image
sh
Copy
Edit
podman tag localhost/docusaurus-image:latest sagarraghuvanshi31/docusaurus-image:latest
🔹 Step 2: Login to Docker Hub
sh
Copy
Edit
podman login docker.io
🔹 Step 3: Push the Image
sh
Copy
Edit
podman push sagarraghuvanshi31/docusaurus-image:latest
🔹 Step 4: Pull the Image
sh
Copy
Edit
docker pull sagarraghuvanshi31/docusaurus-image
🎉 Final Outcome
After successful implementation:

Docusaurus site is live at:
🌍 https://SagarRaghuvanshi31.github.io/my-task/
Containerized version can be pulled using:
sh
Copy
Edit
docker pull sagarraghuvanshi31/docusaurus-image
GitHub Actions ensures automatic deployments
