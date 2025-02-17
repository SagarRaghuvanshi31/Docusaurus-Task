# Docusaurus Setup and Deployment using Podman

## Table of Contents
1. [Objective](#objective)
2. [Proposed Solutions](#proposed-solutions)
   - [Approach 1: Docusaurus Deployment with GitHub Pages and Auto-Refresh](#approach-1-docusaurus-deployment-with-github-pages-and-auto-refresh)
     - [Pros](#pros)
     - [Cons](#cons)
     - [Pre-requisites](#pre-requisites)
   - [Approach 2: Docusaurus Containerization using Podman](#approach-2-docusaurus-containerization-using-podman)
     - [Pros](#pros-1)
     - [Cons](#cons-1)
     - [Pre-requisites](#pre-requisites-1)
3. [Chosen Approach: Approach 2 (Docusaurus Containerization using Podman)](#chosen-approach-approach-2-docusaurus-containerization-using-podman)
   - [Why Choose This Approach?](#why-choose-this-approach)

---

## 1. Objective
The objective of this project is to deploy and manage a Docusaurus documentation site efficiently. The deployment should ensure:

- **Ease of Maintenance** – Automated updates and synchronization with GitHub.
- **Scalability** – The ability to run in a containerized environment.
- **Portability** – Deployable across different platforms using Podman.
- **Automation** – Auto-deployment using GitHub Actions or containerization.

---

## 2. Proposed Solutions
To deploy Docusaurus, two approaches were evaluated:

### **Approach 1: Docusaurus Deployment with GitHub Pages and Auto-Refresh**
This approach involves setting up Docusaurus locally, linking it to a GitHub repository, and using GitHub Actions for automatic deployment to GitHub Pages.

#### **Pros**
✔ Simple and easy to set up.  
✔ Free hosting via GitHub Pages.  
✔ Automated deployment through GitHub Actions.  

#### **Cons**
✖ No built-in containerization, making it harder to run locally in different environments.  
✖ Auto-refresh script runs locally, requiring manual execution.  

#### **Pre-requisites**
**Hardware Requirements:**
- Minimum 2 vCPUs
- 4GB RAM
- 50GB storage

**Software Requirements:**
- Node.js 18+
- Docusaurus
- GitHub Actions

**Networking Requirements:**
- GitHub repository with GitHub Pages enabled.
- Open port 3000 for local development.

---

### **Approach 2: Docusaurus Containerization using Podman**
This approach involves building a Podman image for Docusaurus, running it inside a container, and pushing the image to Docker Hub for deployment.

#### **Pros**
✔ Fully containerized, ensuring portability across different platforms.  
✔ Can be deployed on any server or cloud supporting Podman.  
✔ Isolated environment, reducing dependency conflicts.  

#### **Cons**
✖ Slightly more complex setup due to containerization.  
✖ Requires Podman or Docker to be installed.  

#### **Pre-requisites**
**Hardware Requirements:**
- Minimum 4 vCPUs
- 8GB RAM
- 100GB storage

**Software Requirements:**
- Podman (latest version)
- Node.js 18+
- Docusaurus

**Networking Requirements:**
- Open port 3001 for containerized deployment.
- Docker Hub account for pushing the Podman image.

---

## 3. Chosen Approach: Approach 2 (Docusaurus Containerization using Podman)
### **Why Choose This Approach?**
- **Scalability** – The containerized approach allows deployment on multiple platforms.  
- **Portability** – The same image can be run on any system with Podman.  
- **Automation** – Can be integrated with CI/CD pipelines for seamless updates.  
- **Resource Efficiency** – The lightweight Alpine-based container ensures minimal resource usage.  
