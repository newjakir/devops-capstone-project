## 📚 Project Documentation

This project has two codebases — one for the frontend and another for the backend.

- **Frontend**: A Vue.js Single Page Application (SPA) hosted in its own GitHub repository: [capstoneproject_frontend](https://github.com/newjakir/capstoneproject_frontend).  
- **Backend**: A Django-based REST API project, also hosted in a separate GitHub repository: [capstoneproject_backend](https://github.com/newjakir/capstoneproject_backend).

Each repository includes:  
- CI/CD pipeline definitions (GitHub Actions)  
- Dockerfiles and Docker Compose configurations  
- Entrypoint scripts for service initialization  

The `/kubernetes` directory contains all Kubernetes manifest files required to deploy the complete application stack.

The `/docs` directory contains detailed information about different phases of the project. The information is also available from index below.

---

### 📄 Documentation Index

The following documents provide detailed information about different phases of the project:

- [CI/CD Setup](docs/CI_CD_Setup.md)  
- [Docker Containerization](docs/Docker_Containerization.md)  
- [Kubernetes Architecture](docs/Kubernetes_Architecture.md)  
- [Secret Management](docs/Secret_Management.md)  
- [Monitoring & Logging](docs/Monitoring_Setup.md)