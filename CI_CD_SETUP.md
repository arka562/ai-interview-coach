# CI/CD And Docker Setup

This project has Docker support for local full-stack development and GitHub Actions workflows for CI checks, Docker image publishing, and production deployment.

## 1. Local Docker Development

From the repository root:

```bash
docker compose up --build
```

Services:

```txt
Frontend: http://localhost:5173
Backend:  http://localhost:5000
MongoDB:  localhost:27017
```

The compose setup starts:

- `mongo`: MongoDB 7 with persistent local Docker volume storage.
- `backend`: Express API using `backend/Dockerfile`.
- `frontend`: Vite React app using `frontend/ai_interview_prep/Dockerfile`.

Frontend Dockerfiles:

- `frontend/ai_interview_prep/Dockerfile`: development image used by Docker Compose. It runs Vite dev server on port `5173`.
- `frontend/ai_interview_prep/Dockerfile.prod`: production image used by GHCR publishing. It builds static files and serves them with Nginx on port `80`.

The backend reads `backend/.env` if it exists, then Docker Compose overrides local container-safe values for:

```txt
PORT=5000
JWT_SECRET=docker_dev_secret_change_me
MONGO_URI=mongodb://mongo:27017/ai_interview_prep
CLIENT_URL=http://localhost:5173
CLIENT_URLS=http://localhost:5173,http://127.0.0.1:5173
```

For non-Docker local frontend development, copy the frontend example file:

```bash
cd frontend/ai_interview_prep
cp .env.example .env
```

Use this local value:

```txt
VITE_API_BASE_URL=http://localhost:5000/api/v1
```

Check container status and health:

```bash
docker compose ps
```

Follow logs for all services:

```bash
docker compose logs -f
```

Follow logs for one service:

```bash
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f mongo
```

Stop containers:

```bash
docker compose down
```

Stop containers and remove local MongoDB volume data:

```bash
docker compose down -v
```

## 2. CI Workflow

Workflow file:

```txt
.github/workflows/ci.yml
```

Runs on:

- Push to `main` or `master`.
- Pull requests targeting `main` or `master`.

Jobs:

- Backend: installs dependencies and checks JavaScript syntax.
- Frontend: installs dependencies, runs lint, and builds production assets.
- Docker Builds: builds backend, frontend development, and frontend production Docker images to verify Dockerfiles.

## 3. Docker Image Publishing

Workflow file:

```txt
.github/workflows/docker-publish.yml
```

Runs on:

- Push to `main` or `master`.
- Manual trigger from GitHub Actions.

Publishes images to GitHub Container Registry:

```txt
ghcr.io/arka562/ai-interview-prep-backend:latest
ghcr.io/arka562/ai-interview-prep-frontend:latest
```

It also publishes SHA-tagged images for traceable releases.

The published frontend image uses `frontend/ai_interview_prep/Dockerfile.prod`, so it serves the built React app through Nginx rather than the Vite development server.

This workflow uses GitHub's built-in `GITHUB_TOKEN`, so no Docker Hub token is required.

## 4. Production Deployment

Workflow file:

```txt
.github/workflows/deploy.yml
```

Runs on:

- Push to `main` or `master`.
- Manual trigger from GitHub Actions.

Deploy targets:

- Frontend: Netlify.
- Backend: Render deploy hook.

Required GitHub repository secrets:

```txt
NETLIFY_AUTH_TOKEN
NETLIFY_SITE_ID
VITE_API_BASE_URL
RENDER_DEPLOY_HOOK_URL
```

Where to find them:

- `NETLIFY_AUTH_TOKEN`: Netlify user settings -> Applications -> Personal access tokens.
- `NETLIFY_SITE_ID`: Netlify site settings -> General -> Site details -> Site ID.
- `VITE_API_BASE_URL`: your backend API URL, for example `https://your-render-app.onrender.com/api/v1`.
- `RENDER_DEPLOY_HOOK_URL`: Render web service -> Settings -> Deploy Hook.

If the Netlify or Render secrets are missing, the deploy workflow skips that deploy step instead of failing.

## 5. Recommended Order

1. Run the app locally without Docker.
2. Run the app with Docker Compose.
3. Push to GitHub and confirm `CI` passes.
4. Confirm Docker images publish to GHCR.
5. Add Netlify and Render secrets.
6. Trigger `Deploy` manually once.
7. After the first successful deploy, rely on pushes to `main` for normal deployment.

## 6. Notes

- Do not commit `.env` files.
- Keep AI provider keys in Render environment variables, not in Dockerfiles.
- Keep `VITE_API_BASE_URL` in GitHub secrets and Netlify environment variables.
- Docker Compose is for local development; Render and Netlify remain the production hosting path.
