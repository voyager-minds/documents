--
# Development Guide
# 🚀 CI/CD Pipeline Setup Guide

## 🧑‍💻 Development Environment

### 🖥️ Operating System
Any of the following:
- Windows 10+
- Linux (Fedora/Debian)
- macOS

### 🛠️ IDE
Preferred: **Visual Studio Code**  
Alternative: Any IDE based on developer's preference

### 📦 Frameworks & Packages

| Package   | Version     | Command                          |
|-----------|-------------|----------------------------------|
| Node.js   | 21.x.x      | `npm install v21`               |
| npm       | 10.x.x+     | `npm i -g npm@10`               |
| NestJS    | Nest 10     | `npm install -g @nestjs/cli`    |
| Angular   | 20.x.x      | `npm install -g @angular/cli@20`|
| Git       | Any         | Command line                     |
| VSCode    | Latest      | —                                |

### 🔧 Additional Tools (Install Latest Versions)

| Tool              | Source/Link                        |
|-------------------|------------------------------------|
| Git               | [GitHub](https://github.com/)      |
| MySQL Workbench   | —                                  |
| PuTTY             | —                                  |
| AWS CLI (Optional)| —                                  |
| Notepad++         | —                                  |

---

## 🌐 Language & Frameworks

- **Frontend**: Angular
- **Backend**: Node.js (NestJS) | Java
- **Package Managers**: npm | Maven
- **Version Control**: Git (GitHub as remote)
- **Deployment Architecture**: Docker | Serverless | Static Hosting
- **IDE**: VSCode (with ESLint, Prettier) | IntelliJ

---

## ⚙️ CI Setup

- **Trigger**: On every PR merge to `develop` branch
- **Platform**: GitHub Actions

### 🔁 Pipeline Steps

1. Code Checkout
2. Dependency Install (`npm install` or `mvn install`)
3. Linting: `eslint`
4. Unit Tests:
   - Angular/Node: `jest`
   - Java: `springtest`
5. Security Scans: `Snyk`
6. Build:
   - Compile and bundle (`npm run build`, `ng build`, `cloudFormation`)
7. Artifacts:
   - JS Bundle
   - CloudFormation Stack

---

## 🚀 CD Setup

- **Trigger**: After CI passes
- **Platform**: Docker | GitHub Actions | Serverless

### 📦 Deployment Steps

#### Docker-based Services
- Build Docker image with app and dependencies
- Push to DockerHub

#### Serverless-based Services
- Build CloudFormation Stack
- Upload to S3 bucket
- Attach to CloudFormation and deploy

#### Deploy
- Kubernetes: `kubectl apply -f`
- Serverless: `sls deploy`

#### Post-Deploy Tests
- Smoke tests via curl collection
- API health checks

---

## 🧪 Testing Strategy

| Test Type         | Tools Used       | Frequency              |
|-------------------|------------------|------------------------|
| Linting           | ESLint           | On every commit        |
| Unit Tests        | Jest             | On pull request        |
| Integration Tests | Cypress          | Nightly + Pre-release  |
| E2E Tests         | Cypress          | Staging before prod    |
| Security Scans    | npm audit / Snyk | Weekly or on demand    |
| Load Tests        | JMeter           | Before major releases  |
