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

## ✅ Final Output

The monitoring-enabled Kubernetes deployment has been successfully completed. Below are the accessible endpoints for each component:

### 🔗 Application URLs

- **Frontend Application**: [http://fs.jakirdev.com/](http://fs.jakirdev.com/)
- **Backend API**: [http://finchback.jakirdev.com/](http://finchback.jakirdev.com/)

### 📊 Monitoring Dashboard

- **Grafana Dashboards**: [https://grafanafs.jakirdev.com/](https://grafanafs.jakirdev.com/)
  - **Username**: `admin`
  - **Password**: `admin123`

Grafana dashboards provide:
- Application metrics (frontend and backend)
- Kubernetes cluster health and resource usage
- PostgreSQL and Redis performance
