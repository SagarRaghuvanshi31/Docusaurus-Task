#  Docusaurus Setup, Deployment & Containerization - Test Cases

##  Overview
This document outlines the **test cases** for the setup, deployment, and verification of a **Docusaurus-based documentation site** using:
- **Podman** (for containerization)
- **GitHub Pages** (for hosting)
- **GitHub Actions** (for automated deployment)

Each test case follows the standard **Scenario - Given - When - Then** format for clear execution and verification.

---

## **Test Cases**

## **TC1: Verify Podman Installation**
####  Scenario 
Ensure that **Podman** is installed and accessible.

####  Given  
 A Linux-based system with internet access.


####  When 
The user executes:
```
podman --version
```
###  Then
The installed Podman version should be displayed.
No errors should appear.

### Expected Output
 
podman version 4.3.1

## TC2: Verify Docusaurus Installation
### Scenario
Ensure that Docusaurus is installed and the project is initialized.

#### Given
Node.js is installed.
The user has an active internet connection.
#### When
The user runs:
```
npx create-docusaurus@latest my-task classic
```
#### Then
The project should be created in the my-task directory.

The required project structure should be generated.
#### Expected Output

 Success! Created my-task in ./my-task
 
## TC3: Verify Git Repository Initialization
#### Scenario
Ensure Git is initialized in the Docusaurus project.

#### Given
Git is installed.
#### When
The user runs:
```
cd my-task
git init
```
#### Then
The repository should be successfully initialized.
#### Expected Output
Initialized empty Git repository in /home/user/my-task/.git/


## TC4: Validate GitHub Repository Connection
#### Scenario
Ensure that the local repository is correctly linked to a GitHub repository.

#### Given
A GitHub repository is created.
The repository URL is known.
#### When
The user runs:
```
git remote -v
```
#### Then
The correct GitHub repository URL should be displayed.
#### Expected Output
origin https://github.com/SagarRaghuvanshi31/my-task.git (fetch)

origin https://github.com/SagarRaghuvanshi31/my-task.git (push)

## TC5: Verify Local Docusaurus Server Startup
#### Scenario
Ensure that the Docusaurus development server starts successfully.

#### Given
The Docusaurus project has been initialized.
#### When
The user executes:
```
npm start
```
#### Then
The site should be accessible at http://localhost:3000.
No errors should occur.
#### Expected Output
Docusaurus website is running at http://localhost:3000/

## TC6: Verify GitHub Pages Deployment
#### Scenario
Ensure that the Docusaurus site is successfully deployed to GitHub Pages.

#### Given
The docusaurus.config.js file is configured correctly.
The GitHub Pages branch is enabled.
#### When
The user visits:
https://SagarRaghuvanshi31.github.io/my-task/
#### Then
The deployed site should load correctly.

## TC7: Verify Podman Image Build
#### Scenario
Ensure the Docusaurus container image is built using Podman.

#### Given
A valid Dockerfile is present.
#### When
The user runs:
```
podman build -t docusaurus-image .
```
#### Then
The image should be successfully created.
#### Expected Output
Successfully built docusaurus-image

## TC8: Verify Docusaurus Container Runs in Podman
#### Scenario
Ensure that the Docusaurus container starts successfully.

#### Given
A Podman image is available.
#### When
The user executes:
```
podman run -d -p 3001:3000 --name docusaurus-container docusaurus-image
```
#### Then
The site should be accessible at http://localhost:3001.

## TC9: Validate GitHub Actions Workflow Deployment
#### Scenario
Ensure that GitHub Actions successfully builds and deploys Docusaurus.

#### Given
A valid .github/workflows/deploy.yml file exists.
#### When
The user pushes:
```
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow"
git push origin main
```
#### Then
GitHub Actions should trigger the deployment.

## TC10: Verify Auto-Refresh Script Execution
#### Scenario
Ensure that Docusaurus automatically restarts when changes are detected.

#### Given
A valid auto_refresh.sh script exists.
#### When
The user runs:
```
nohup ~/my-task/auto_refresh.sh > ~/my-task/refresh.log 2>&1 &
```
#### Then
The site should restart automatically upon file changes.

## TC11: Validate GitHub Pages Configuration
#### Scenario
Ensure that docusaurus.config.js is correctly set for GitHub Pages.

#### Given
A valid Docusaurus project.
#### When
The user reviews docusaurus.config.js.

#### Then
The file should contain:
url: 'https://SagarRaghuvanshi31.github.io',
baseUrl: '/my-task/',
deploymentBranch: 'gh-pages',

## TC12: Verify Docker Image Push to Docker Hub
#### Scenario
Ensure that the Docusaurus container image is pushed to Docker Hub.

#### Given
A valid Podman image exists.
#### When
The user executes:
```
podman push sagarraghuvanshi31/docusaurus-image:latest
```
#### Then
The image should be successfully uploaded to Docker Hub.
