
# Guide to Setting Up a CI/CD Pipeline Using GitHub Actions

## Introduction

GitHub Actions is a powerful tool for automating software workflows, including Continuous Integration (CI) and Continuous Deployment (CD). This guide walks you through setting up a CI/CD pipeline using GitHub Actions.

---

## Prerequisites

1. **GitHub Repository**: Ensure you have a GitHub repository for your project.
2. **Basic Knowledge of Git**: Familiarity with Git commands and workflows.
3. **Application Code**: Your application code should be ready for deployment.
4. **Deployment Environment**: A hosting service like AWS, Azure, GCP, or GitHub Pages.

---

## Steps to Set Up CI/CD Pipeline

### 1. **Create a `.github/workflows` Directory**

1. In your project root, create the `.github` directory:
   ```bash
   mkdir -p .github/workflows
   ```
2. Inside the `workflows` folder, create a YAML file (e.g., `ci-cd.yml`):
   ```bash
   touch .github/workflows/ci-cd.yml
   ```

### 2. **Define Your Workflow**

Open the `ci-cd.yml` file and define your pipeline. Below is an example for a Node.js application:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Deploy Application
        env:
          API_KEY: ${{ secrets.DEPLOY_API_KEY }}
        run: |
          # Example deployment script
          echo "Deploying application..."
```

### 3. **Add Secrets**

1. Navigate to your GitHub repository.
2. Go to **Settings > Secrets and variables > Actions**.
3. Add necessary secrets, such as API keys, tokens, or passwords, required for deployment.

### 4. **Test the Workflow**

1. Push your code changes to the `main` branch:
   ```bash
   git add .
   git commit -m "Add CI/CD pipeline"
   git push origin main
   ```
2. Navigate to the **Actions** tab in your GitHub repository.
3. Monitor the workflow execution and check for errors.

### 5. **Customize as Needed**

Modify the workflow to fit your project's requirements:
- Adjust Node.js version or dependencies.
- Add deployment scripts specific to your hosting service.
- Include additional jobs for linting, building Docker images, or other tasks.

---

## Common Use Cases

1. **Node.js Application**: Run tests, build artifacts, and deploy.
2. **Python Application**: Use `actions/setup-python` for environment setup.
3. **Docker Applications**: Build and push Docker images.
4. **Static Sites**: Deploy to GitHub Pages or other static hosting services.

---

## Conclusion

With GitHub Actions, setting up a CI/CD pipeline is straightforward and scalable. Experiment with additional workflows to automate more aspects of your development and deployment processes.

---

## Resources

- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Marketplace Actions](https://github.com/marketplace?type=actions)
