# Deploying to Coolify

This guide explains how to deploy your ERP system (Next.js Frontend + Flask Backend) to Coolify.

---

## Deployment Options

You can deploy the application using two approaches in Coolify:
1. **Docker Compose Deployment (Single Application)**: Deploy both services together using the root [docker-compose.yml](file:///c:/Users/sriram/Downloads/erp-prd/docker-compose.yml).
2. **Separate Deployments (Two Coolify Applications)**: Deploy the backend and frontend separately for independent scaling, build controls, and health monitoring.

---

### Option 1: Docker Compose (Recommended for Simplicity)

1. In Coolify, create a **New Resource** and choose **Docker Compose**.
2. Point Coolify to your Git Repository.
3. Coolify will read the [docker-compose.yml](file:///c:/Users/sriram/Downloads/erp-prd/docker-compose.yml) in the root.
4. Set the **Environment Variables** in the Coolify configuration panel (see below).
5. Deploy the application. Coolify will automatically configure the routing and expose the ports.
6. Configure domains in the Coolify UI:
   - Bind a domain for the Frontend (mapped to port `3000`).
   - Bind a domain for the Backend (mapped to port `7860`).

---

### Option 2: Separate Deployments (Recommended for Production)

If you prefer to scale or manage the frontend and backend independently:

#### A. Backend Service
1. Create a **New Resource** in Coolify -> **Public/Private Repository**.
2. Point it to your Git repository.
3. Set the **Base Directory** to `backend`.
4. Coolify will auto-detect the [Dockerfile](file:///c:/Users/sriram/Downloads/erp-prd/backend/Dockerfile) in the `backend` folder.
5. Set the required **Environment Variables** (`DATABASE_URL`, `SECRET_KEY`, `JWT_SECRET_KEY`).
6. Set the application port to `7860`.
7. Bind your API domain (e.g. `https://api.yourdomain.com`).

#### B. Frontend Service
1. Create a **New Resource** in Coolify -> **Public/Private Repository**.
2. Point it to your Git repository.
3. Set the **Base Directory** to `frontend`.
4. Coolify will auto-detect the [Dockerfile](file:///c:/Users/sriram/Downloads/erp-prd/frontend/Dockerfile) in the `frontend` folder.
5. Set the application port to `3000`.
6. Add the build-time argument `NEXT_PUBLIC_API_URL` (e.g. `https://api.yourdomain.com/api`).
7. Bind your main domain (e.g. `https://yourdomain.com`).

---

## Environment Variables Configuration

| Variable Name | Required At | Description | Example |
|---|---|---|---|
| `DATABASE_URL` | Runtime (Backend) | PostgreSQL connection string | `postgresql://postgres:password@host:5432/db` |
| `SECRET_KEY` | Runtime (Backend) | Flask application secret key | `super-secret-key-string` |
| `JWT_SECRET_KEY` | Runtime (Backend) | Flask JWT token secret key | `jwt-secret-key-string` |
| `NEXT_PUBLIC_API_URL` | **Build & Runtime** (Frontend) | Public URL of the Flask API. Must end in `/api` | `https://api.yourdomain.com/api` |

> [!WARNING]
> Next.js embeds environment variables starting with `NEXT_PUBLIC_` into the client-side JavaScript bundle during the build phase (`next build`). 
> Therefore, you **MUST** configure `NEXT_PUBLIC_API_URL` under both the **Build Arguments** and **Environment Variables** in Coolify for it to be accessible by client browsers.
