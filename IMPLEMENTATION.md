# Docusaurus Setup and Deployment with Gitea Webhook

## 1. Install & Set Up Docusaurus Locally

Run the following commands to create a new Docusaurus project and navigate into it:
```
npx create-docusaurus@latest my-task classic
cd my-task
npm install
```
This will create a Docusaurus project inside the my-task folder.

## 2. Set Up Gitea & Push Code to Gitea Repository

### Step 1: Install Gitea

If you haven't installed Gitea yet, you can do so by running:
```
sudo apt update
sudo apt install gitea
```

### Start Gitea:

```
sudo systemctl enable --now gitea
```
Now, open your browser and go to http://localhost:3001/ to complete the Gitea setup.

### Step 2: Create a Repository in Gitea

Log in to your Gitea instance.
Click on "New Repository".
Set the repository name as my-task.
Click Create Repository.

### Step 3: Initialize Git and Push Code

```
cd my-task
git init
git remote add origin http://localhost:3001/Sagar31/my-task.git
git branch -M main
git add .
git commit -m "Initial Docusaurus setup"
git push -u origin main
```

## 3. Update Docusaurus Configuration (docusaurus.config.js)

Now, update the docusaurus.config.js file inside the my-task folder:

```
// @ts-check
import { themes as prismThemes } from 'prism-react-renderer';

/** @type {import('@docusaurus/types').Config} */
const config = {
  title: 'Meri Docs Site',
  url: 'http://localhost:3000',
  baseUrl: '/my-task/',
  organizationName: 'Sagar31',
  projectName: 'my-task',
  deploymentBranch: 'gh-pages',

  onBrokenLinks: 'throw',
  onBrokenMarkdownLinks: 'warn',
  i18n: { defaultLocale: 'en', locales: ['en'] },

  presets: [
    [
      'classic',
      {
        docs: { sidebarPath: require.resolve('./sidebars.js') },
        blog: { showReadingTime: true },
        theme: { customCss: require.resolve('./src/css/custom.css') },
      },
    ],
  ],

  themeConfig: {
    navbar: {
      title: 'Meri Docs Site',
      logo: { alt: 'Logo', src: 'img/logo.svg' },
      items: [
        { type: 'docSidebar', sidebarId: 'tutorialSidebar', label: 'Tutorial', position: 'left' },
        { to: '/blog', label: 'Blog', position: 'left' },
        { href: 'http://localhost:3001/Sagar31/my-task', label: 'Gitea', position: 'right' },
      ],
    },
    footer: {
      style: 'dark',
      links: [{ title: 'Docs', items: [{ label: 'Tutorial', to: '/docs/intro' }] }],
      copyright: `(c) ${new Date().getFullYear()} Built with Docusaurus.`,
    },
    prism: { theme: prismThemes.github, darkTheme: prismThemes.dracula },
  },
};

export default config;
```

### Push Changes to Gitea

```
git add docusaurus.config.js
git commit -m "Update Docusaurus config for Gitea"
git push origin main
```

## 4. Set Up Gitea Webhook for Auto Deployment

### Step 1: Create Webhook in Gitea

Go to your Gitea repository (http://localhost:3001/Sagar31/my-task).
Click on Settings → Webhooks → Add Webhook.
Select Gitea.

### In the Target URL, enter:

http://your-server-ip:3002/webhook
Select Trigger on push events.
Click Add Webhook.

## Step 2: Set Up Webhook Server

To handle webhook events, set up a simple Flask server:

```
sudo apt install python3-flask -y
nano webhook_server.py
```

Paste this code:

```
import os
import subprocess
from flask import Flask, request

app = Flask(__name__)

REPO_PATH = "/home/youruser/my-task"  # Change this to the correct path

@app.route('/webhook', methods=['POST'])
def webhook():
    try:
        subprocess.run(['git', '-C', REPO_PATH, 'pull', 'origin', 'main'], check=True)
        return "Updated Successfully", 200
    except subprocess.CalledProcessError as e:
        return f" Error: {str(e)}", 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3002)
```

### Save and start the server:

```
python3 webhook_server.py &
```
Now, every time you push to Gitea, the webhook will trigger an automatic update.

## 5. Enable Gitea Pages for Deployment

Go to Gitea Repository → Settings → Pages.
Select the gh-pages branch under "Source".
Click Save.

## 6. Containerizing Docusaurus with Podman

### Step 1: Create a Dockerfile

```
nano Dockerfile
```

Add this:

```
# Base Image
FROM node:18-alpine

# Working directory
WORKDIR /app

# Copy project files
COPY . .

# Install dependencies
RUN npm install

# Expose port
EXPOSE 3000

# Start Docusaurus with 0.0.0.0 binding
CMD ["npm", "start", "--", "--host", "0.0.0.0"]
```

### Step 2: Build the Podman Image

```
podman build -t docusaurus-image .
```

### Step 3: Run the Container

```
podman run -d -p 3000:3000 --name docusaurus-container docusaurus-image
```

Now, open http://localhost:3000 in your browser to see Docusaurus running inside the container.

### Step 4: Push Image to Docker Hub

```
podman login docker.io
podman tag localhost/docusaurus-image:latest Sagar31/docusaurus-image:latest
podman push Sagar31/docusaurus-image:latest
```

### Step 5: Pull Image

```
docker pull Sagar31/docusaurus-image
```

