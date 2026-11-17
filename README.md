# thinktwicev2

Fresh full-stack starter based on the approved two-screen ThinkTwice Cerulean prototype.

## Project layout
- `frontend/`: Vite static frontend; dark/light mode, font scaling, language UI, voice input, text/link/image checks, dedicated report screen, sources, explanation, next steps, share, local history.
- `backend/`: FastAPI API with optional Tavily source search and OpenAI-based assessment.
- `render.yaml`: Render backend deployment blueprint.

## Run locally in VS Code

Open the `thinktwicev2` folder in VS Code. Open **two terminals**:

**Terminal 1 — backend**
```bash
cd backend
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # Windows PowerShell: Copy-Item .env.example .env
# Edit backend/.env with your own keys.
uvicorn app.main:app --reload --port 8000
```

**Terminal 2 — frontend**
```bash
cd frontend
npm install
cp .env.example .env   # Windows PowerShell: Copy-Item .env.example .env
npm run dev
```

Open http://localhost:5173. The **Try example report** button works without API keys. Real checks need OPENAI_API_KEY; search-backed evidence URLs need TAVILY_API_KEY.

## Git setup
```bash
git init
git add .
git commit -m "Initial ThinkTwice v2 project"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/thinktwicev2.git
git push -u origin main
```
Create an empty private GitHub repo first. Do not commit `.env` or API keys.

## Render deployment
1. Connect the new GitHub repo to Render; create a Blueprint using `render.yaml` (or a Python web service with root `backend`).
2. Add secret environment variables in Render: `OPENAI_API_KEY`, `TAVILY_API_KEY` (optional for sources), `CORS_ORIGINS` (your Vercel URL), `FRONTEND_URL` (your Vercel URL).
3. Deploy and check `https://YOUR-RENDER-URL/health`.

## Vercel deployment
1. Import the same GitHub repo into Vercel.
2. Set **Root Directory** to `frontend` and framework to **Vite**.
3. Set `VITE_API_BASE_URL=https://YOUR-RENDER-URL` (without trailing `/api`).
4. Deploy, then set Render `CORS_ORIGINS` and `FRONTEND_URL` to the exact Vercel origin and redeploy Render.

## Security and feature notes
- Do not put API keys in frontend variables; Vite `VITE_` variables are publicly visible.
- Demo data is illustrative and not a verified claim.
- This is a new backend implementation, **not** a complete copy of the old backend. Google login, persistent server-side history, feedback, and database migrations are **not included**; recent checks are stored locally in the browser.
- Interface localization covers the home screen; AI-generated report text is not translated.
- Search results are only leads; their presence does not establish a claim as true. If Tavily is not configured, verdict is conservatively Unverified.
- For production, add rate limiting, abuse protection, DNS/redirect safeguards if URL fetching is added, privacy policy, and monitoring before public launch.
- This package is not deployed and has no live keys.
