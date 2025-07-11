# Docker Containerization

This document describes how the **finch‑backend (Django)** and **finch‑frontend (Vue + NGINX)** Docker images are built, optimized, and run locally.  
It also covers the purpose of the **.dockerignore** files and provides an optional **docker‑compose.yml** for streamlined local development.

## 🐍 Backend Image (Django)

### Multi‑Stage Dockerfile (`backend/Dockerfile`)

| Stage        | Base Image     | Key Actions                                               | Why                                                         |
|--------------|----------------|-----------------------------------------------------------|-------------------------------------------------------------|
| **builder**  | `ubuntu:22.04` | • Installs full build tool‑chain and dev headers <br>• Creates Python 3 virtualenv (`venv1`) <br>• Installs requirements with `--no‑cache‑dir` | Keeps dependencies isolated; avoids polluting runtime image |
| **production** | `ubuntu:22.04` | • Installs minimal runtime libs (`python3`, `libpq5`) <br>• Copies pre‑built `venv1` from builder <br>• Adds app code & non‑root `appuser` <br>• Starts via custom `entrypoint.sh` | Shrinks final image; drops build‑time tooling for security  |

#### Image‑Size & Security Optimizations

* **Multi‑Stage Build** removes compilers and dev libraries from the final layer.  
* `apt-get clean && rm -rf /var/lib/apt/lists/*` clears APT cache.  
* `pip install --no-cache-dir` avoids storing wheels.  
* Runs as **non‑root** (`appuser`).  
* All mutable data (`db.sqlite3`, logs, uploads) can be mapped to volumes at runtime.  
* `ENTRYPOINT` script handles migrations + data import, making the container truly 12‑factor capable.

---

## 🌐 Frontend Image (Vue.js + NGINX)

### Multi‑Stage Dockerfile (`frontend/Dockerfile`)

| Stage        | Base Image     | Key Actions                            | Why                                              |
|--------------|----------------|----------------------------------------|--------------------------------------------------|
| **build**    | `node:18-alpine` | • Installs dependencies with `npm ci` <br>• Executes `npm run build` to produce static assets | Alpine image is tiny; `npm ci` uses cached lockfile |
| **serve**    | `nginx:alpine`   | • Copies `/app/dist` → `/usr/share/nginx/html` <br>• Adds production `nginx.conf` <br>• Optional `entrypoint.sh` for env‑injection | NGINX is battle‑tested, small, and static‑file‑optimized |

#### Image‑Size & Security Optimizations

* **Alpine** base yields ~ 25 MB layers.  
* Build cache preserved across runs by copying only `package*.json` first.  
* No Node runtime lives in the final image → smaller attack surface.  
* `ENTRYPOINT` kept minimal; main process stays as PID 1 (`nginx`).  

---

## 🚫 .dockerignore

Both repos include a `.dockerignore` file to prevent large or sensitive files from inflating the build context:

