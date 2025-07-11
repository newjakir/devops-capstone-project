# 🔐 Secret Management in Kubernetes

This document outlines how sensitive information such as database credentials and API keys are securely managed within our Kubernetes environment using **Kubernetes-native Secrets**.

---

## 📌 Secret Management Strategy

### ✅ Chosen Approach: Kubernetes Secrets

We are using **Kubernetes Secrets** to manage sensitive configuration values in the cluster. This ensures credentials are:
- Not hardcoded in configuration files or container images.
- Mounted or injected securely into application pods at runtime.
- Version-controlled only as base64‑encoded manifests **without real values** committed to Git.

---

## 📁 Secret Manifest Files

The following files store the structure of our secrets (with placeholder/base64-encoded values):

./kubernetes/secrets/
├── db-credentials.yaml # Database username & password
└── api-keys.yaml # Stripe keys, site URLs, etc.


---

## 🔐 Example: `db-credentials.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
  namespace: your-namespace
type: Opaque
data:
  POSTGRES_USER: cG9zdGdyZXM=              # postgres (base64)
  POSTGRES_PASSWORD: c2VjdXJlcGFzc3dvcmQ=   # securepassword (base64)
  POSTGRES_DB: ZmluY2hkYg==                # finchdb (base64)

apiVersion: v1
kind: Secret
metadata:
  name: api-keys
  namespace: your-namespace
type: Opaque
data:
  STRIPE_SECRET_KEY: c2stdGVzdC1rZXktMTIz
  STRIPE_PUBLIC_KEY: cGtfdGVzdF9rZXktNDU2
  SITE_URL: aHR0cDovL2xvY2FsaG9zdDo4MDAw
  FRONTEND_SITE_URL: aHR0cDovL2xvY2FsaG9zdDo4MA==

