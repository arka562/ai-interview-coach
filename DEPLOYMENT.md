# Deployment: Netlify + Render

## Backend on Render

1. Push this repository to GitHub.
2. In Render, create a new Web Service from the GitHub repo.
3. Set the root directory to `backend`.
4. Use these settings:
   - Runtime: Node
   - Build command: `npm install`
   - Start command: `npm start`
   - Instance type: Free
5. Add environment variables from `backend/.env.example`.
6. After deploy, copy the Render public URL.

Required backend variables:

```txt
MONGO_URI=your MongoDB Atlas connection string
JWT_SECRET=a long random secret
CLIENT_URL=https://your-netlify-site.netlify.app
```

Add at least one AI provider key for question generation/evaluation:

```txt
GEMINI_API_KEY=...
```

## Frontend on Netlify

1. In Netlify, create a new site from the same GitHub repo.
2. Set the base directory to `frontend/ai_interview_prep`.
3. Netlify reads `netlify.toml`, but these are the values:
   - Build command: `npm run build`
   - Publish directory: `dist`
4. Add this environment variable:

```txt
VITE_API_BASE_URL=https://your-render-backend.onrender.com/api/v1
```

5. Deploy the frontend.
6. Copy the Netlify URL and set it as `CLIENT_URL` in Render.
7. Redeploy/restart the Render backend after changing `CLIENT_URL`.

## GitHub Actions CI/CD

This repo includes workflows for CI checks, Docker image publishing, and optional production deploys.

For the full local Docker and CI/CD guide, see `CI_CD_SETUP.md`.

Required GitHub repository secrets for production deploy:

```txt
NETLIFY_AUTH_TOKEN=your Netlify personal access token
NETLIFY_SITE_ID=your Netlify site API ID
VITE_API_BASE_URL=https://your-render-backend.onrender.com/api/v1
RENDER_DEPLOY_HOOK_URL=your Render deploy hook URL
```

How to find the values:

- Netlify auth token: Netlify user settings -> Applications -> Personal access tokens.
- Netlify site ID: Netlify site settings -> General -> Site details -> Site ID.
- Render deploy hook: Render web service -> Settings -> Deploy Hook.

After these secrets are added, pushes to `main` or `master` will:

- Run CI checks.
- Build and publish Docker images to GitHub Container Registry.
- Build and deploy the frontend to Netlify.
- Trigger a backend redeploy on Render.

## MongoDB Atlas

Make sure Render can connect:

- Database user/password are correct.
- Network access allows Render. For a demo project, `0.0.0.0/0` is the simplest setting.

## Local Development

Frontend:

```bash
cd frontend/ai_interview_prep
npm install
npm run dev
```

Backend:

```bash
cd backend
npm install
npm run dev
```
