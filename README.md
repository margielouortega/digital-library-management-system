# HEU Digital Library Management System

React + Vite frontend and Node.js + Express backend. The backend stores data in a JSON file.

## Run locally in VS Code

Open this folder in VS Code. Use two terminals:

**Terminal 1 — backend**
```bash
cd backend
npm install
npm start
```
API: `http://localhost:5000/api`  
Health check: `http://localhost:5000/api/health`

**Terminal 2 — frontend**
```bash
cd frontend
npm install
npm run dev
```
Open the Vite address, normally `http://localhost:5173`.

## Demo accounts
- Librarian: `librarian@library.local` / `Admin123!`
- Student: `student@library.local` / `Student123!`

## Student interface update
The green, white and yellow student theme is scoped to the student Dashboard, Catalog Items, Categories and Circulation History pages. Librarian pages and the existing library functions are retained. The student can browse books, search the catalog/categories, and view their own borrowing/return history.

## Deploy backend to Render

1. Push the complete `digital-library-management-system` folder to a GitHub repository.
2. In Render, choose **New + → Blueprint** and connect that repository, or create a **Web Service** manually.
3. If creating manually, set:
   - Root Directory: `backend`
   - Build Command: `npm install`
   - Start Command: `npm start`
4. Add environment variables:
   - `JWT_SECRET`: a long random secret (Render can generate one)
   - `CLIENT_ORIGIN`: your deployed Vercel URL, for example `https://your-site.vercel.app`
5. Deploy. Test `https://YOUR-RENDER-SERVICE.onrender.com/api/health`. It should return JSON with `"status":"ok"`.

The included `render.yaml` provides the Render Blueprint configuration. Set `CLIENT_ORIGIN` in the Render dashboard after deployment if using the Blueprint.

## Deploy frontend to Vercel

1. In Vercel, choose **Add New → Project** and import the same GitHub repository.
2. Set **Root Directory** to `frontend`.
3. Framework preset: **Vite**. Build command: `npm run build`. Output directory: `dist`.
4. Add environment variable:
   - `VITE_API_URL`: `https://YOUR-RENDER-SERVICE.onrender.com/api`
5. Deploy or redeploy after adding/changing the environment variable.

The `frontend/vercel.json` file includes the SPA rewrite configuration. If you use a different Vercel domain, update Render's `CLIENT_ORIGIN` to that exact HTTPS origin and redeploy the backend.

## Important deployment notes

- Vercel frontend and Render backend are separate deployments. They connect through `VITE_API_URL`.
- `VITE_API_URL` must end in `/api` and must not include an extra route such as `/health`.
- This classroom version uses a local JSON file (`backend/data/library-data.json`). Render's ordinary container filesystem is not durable across all redeploys/restarts. For records that must persist in production, attach a Render persistent disk mounted at the backend data directory or migrate the storage to a managed database. Do not use this JSON-file storage for sensitive real student records without appropriate security, backups, and privacy review.
- Change demo passwords before sharing the deployed app.
- Keep `.env` files and secrets out of GitHub. Only commit `.env.example` files.
