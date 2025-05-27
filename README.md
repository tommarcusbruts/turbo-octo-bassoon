# Node.js Demo App (DevOps Internship Task 2)

This is a basic Node.js Express app used for demonstrating CI/CD using GitHub Actions and Docker.

---

## 🚀 Task: Automate Code Deployment using CI/CD

### ✅ Objective
Set up a GitHub Actions workflow that builds a Docker image of this app and pushes it to DockerHub on every push to the `main` branch.

### 🔧 Tools Used
- GitHub Actions
- Docker & DockerHub
- Node.js

### ⚙️ CI/CD Workflow
1. Triggered on push to `main`
2. Installs dependencies
3. Builds Docker image
4. Pushes image to DockerHub

### 📂 File Structure
.github/workflows/main.yml
Dockerfile
index.js
package.json
