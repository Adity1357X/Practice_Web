# AI Quiz & Self-Training App

Self-paced (no timer) quiz trainer. Frontend → Express backend → Groq AI → validated JSON → interactive practice.

## Setup
```powershell
cd quiz-app
cp .env.example .env   # then edit AI_API_KEY (or copy key from Practice/Groq.txt into server .env)
npm run install:all
```
Copy the key into `server/.env` too (server reads `server/.env`? No — root `.env` is loaded via dotenv cwd; simplest: put `.env` in BOTH root and `server/` or set env var). Easiest:
```powershell
Copy-Item .env.example server\.env
# edit server\.env -> AI_API_KEY=...
```

## Run (one command)
```powershell
cd quiz-app
npm run dev     # builds frontend + serves everything at http://localhost:3001
```
Open **http://localhost:3001** — frontend and API run from this single server.

Hot-reload dev (two terminals) instead:
```powershell
npm run dev:server   # http://localhost:3001 (/api/health)
npm run dev:client   # http://localhost:5173 (proxies /api to :3001)
```
Vite proxies `/api` → `localhost:3001`. Or set `VITE_API_URL=http://localhost:3001` in `client/.env`.

If the AI is unreachable the server returns a clear error (no demo content — every quiz is real AI-generated).

## Config
- Classes/subjects/chapters: `client/src/data/curriculum.js`
- Models: `ALLOWED_MODELS` in `server/.env`, dropdown in Create + Settings
- Storage: `localStorage` (`quizapp.*`), ready to swap for Firebase later

## Pushing to GitHub safely
Secrets stay out of git automatically: `.gitignore` blocks `*.env`, `Groq.txt`, `node_modules/`, `dist/`. Only `.env.example` (no real key) is committed.
```powershell
cd quiz-app
git init; git add .; git status      # confirm NO .env file is listed
git commit -m "AI quiz app"
gh repo create ai-quiz-app --private --source=. --push   # or push to your remote
```
After cloning elsewhere: `Copy-Item .env.example server\.env`, add your key, `npm run install:all`, `npm run dev`.
API protections: key server-side only, per-IP rate limit (`GEN_LIMIT_PER_HOUR`), optional origin lock (`ALLOWED_ORIGINS`), small JSON body cap, no `X-Powered-By` header.
