# CI/CD Pipeline Setup

This document describes the continuous‑integration and continuous‑deployment (CI/CD) configuration for the **finch‑backend (Django)** and **finch‑frontend (Vue.js)** applications.  
Both pipelines are implemented with **GitHub Actions** and automate linting, testing, building, and pushing Docker images.

---

## 🛠 Chosen CI/CD Tool

| Tool            | Why we chose it                                                     |
|-----------------|---------------------------------------------------------------------|
| **GitHub Actions** | • Native GitHub integration  <br>• First‑class Docker support  <br>• Matrix builds and caching <br>• Generous free minutes for public repos |

---

## 🔄 Pipeline Overview

| Stage                   | Backend (Django) | Frontend (Vue) | Purpose                                           |
|-------------------------|------------------|----------------|---------------------------------------------------|
| **Checkout**            | ✅               | ✅             | Pull repository code                              |
| **Lint**                | `flake8`         | `eslint`       | Enforce code quality                              |
| **Unit / Integration Tests** | `pytest` / Django test runner inside Docker | `vue-cli-service test:unit` | Validate business logic                           |
| **Build Artifacts**     | Not required (Python interpreted) | `npm run build` | Produce optimized frontend bundle                |
| **Docker Build**        | 🐳 Build image   | 🐳 Build image | Containerize the app                              |
| **Docker Push**         | Push to Docker Hub | Push to Docker Hub | Publish image for deployment                      |

---

## ⚙️ Backend Workflow (`.github/workflows/backend-ci.yml`)

### Key Jobs

1. **lint**  
   * Install `flake8`, ignore `venv` & `migrations`, exit‑zero (warnings don’t fail build).

2. **test** (depends on **lint**)  
   * Starts services via `docker compose up -d --build`.  
   * Waits until PostgreSQL is healthy.  
   * Runs Django tests inside the `3-tire-back` container.  
   * Tears down containers on completion.

3. **docker-build-and-push** (depends on **test**)  
   * Logs in to Docker Hub.  
   * Builds `newjakir/3-tire-back:latest` and pushes it.

### Required Secrets / Variables

| Name | Notes |
|------|-------|
| `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` | Database credentials |
| `STRIPE_SECRET_KEY`, `STRIPE_PUBLIC_KEY`, `STRIPE_WEBHOOK_SECRET` | Payment integration |
| `SITE_URL`, `FRONTEND_SITE_URL` | CORS / callbacks |
| `DJANGO_SUPERUSER_USERNAME`, `DJANGO_SUPERUSER_EMAIL`, `DJANGO_SUPERUSER_PASSWORD` | Superuser bootstrap |
| `DOCKER_USERNAME`, `DOCKER_PASSWORD` | Docker Hub login |

---

## 🌐 Frontend Workflow (`.github/workflows/frontend-ci.yml`)

### Key Jobs

1. **build-and-test**  
   * Installs dependencies with `npm ci`.  
   * Runs ESLint (`npm run lint`).  
   * Executes unit + integration tests (`npm run test:unit`).  
   * Builds production bundle (`npm run build`).

2. **docker** (depends on **build-and-test**)  
   * Authenticates with Docker Hub.  
   * Builds `newjakir/capsp-frontend:latest`.  
   * Pushes image.

### Required Secrets / Variables

| Name | Notes |
|------|-------|
| `VUE_APP_API_URL` | Backend base URL during tests |
| `VUE_APP_STRIPE_PUBLIC_KEY` | Stripe publishable key |
| `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN` | Docker Hub login |

---

## 🚀 Triggers

Both workflows run automatically on:

* **Push** to `main`
* **Pull Request** targeting `main`

No extra configuration is needed. Push commits or open PRs to start the pipeline.

---

## 📈 How to Monitor Runs

1. **GitHub UI**  
   * Navigate to **Actions** tab in the repo.  
   * Select **Backend CI/CD** or **Frontend CI/CD** workflow.  
   * Click a run to inspect step‑by‑step logs.

2. **GitHub CLI** (optional)  
   ```bash
   gh run list               # show recent runs
   gh run view <run-id>      # show details & logs