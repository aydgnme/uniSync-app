# UniSync App

Monorepo that hosts the **UniSync** mobile frontend (Expo/React Native) and the **UniSync Backend** (Fastify + TypeScript) side-by-side.  
This repository is a thin orchestration layer: the `frontend/` and `backend/` folders are brought in as **Git subtrees** from their own upstream repositories.

- Frontend upstream: `https://github.com/aydgnme/uniSync`
- Backend upstream:  `https://github.com/aydgnme/uniSync-backend`

> Use this repo if you want to clone, run, develop and deploy **both** apps together, or if you prefer a single remote for CI/CD.

---

## Repository Layout
```
uniSync-app/
├─ frontend/   # Expo (React Native) app (subtree of aydgnme/uniSync)
├─ backend/    # Fastify + TypeScript API (subtree of aydgnme/uniSync-backend)
└─ README.md
---
```

## Prerequisites

- **Git** ≥ 2.30 (subtree support)
- **Node.js** ≥ 18.x and **npm** ≥ 9
- **Java JDK** & Android SDK (for Android emulator) / Xcode (for iOS simulator)
- **Docker** (optional, to run the backend via container)

---

## First-time Setup

```bash
# 1) Clone this repo
git clone https://github.com/aydgnme/uniSync-app
cd uniSync-app

# 2) Add FRONTEND as subtree (squashed history)
git subtree add --prefix=frontend https://github.com/aydgnme/uniSync.git main --squash

# 3) Add BACKEND as subtree (squashed history)
git subtree add --prefix=backend https://github.com/aydgnme/uniSync-backend.git main --squash
```
 ---

 ## Environment Variables

### frontend/.env
```.env
EXPO_PUBLIC_API_BASE_URL=http://localhost:3000
EXPO_PUBLIC_ENV=local
```
### backend/.env
```.env
PORT=3000
NODE_ENV=development
DATABASE_URL=postgres://postgres:postgres@localhost:5432/unisync
SUPABASE_URL=https://YOUR-PROJECT.supabase.co
SUPABASE_ANON_KEY=YOUR-ANON-KEY
SUPABASE_SERVICE_ROLE_KEY=YOUR-SERVICE-ROLE-KEY
JWT_SECRET=change-me
```
---

## Running Locally

### Backend
```bash
cd backend
npm install
npm run dev
# server on http://localhost:3000
```
Or with Docker:
```bash
docker build -t unisync-backend .
docker run --env-file .env -p 3000:3000 unisync-backend
```
### Frontend
```bash
cd frontend
npm install
npx expo start
# Press i (iOS) / a (Android) or scan QR with Expo Go
```

---

## Keeping Subtrees in Sync

### Pull latest upstream changes:
```bash
git subtree pull --prefix=frontend https://github.com/aydgnme/uniSync.git main --squash
git subtree pull --prefix=backend https://github.com/aydgnme/uniSync-backend.git main --squash
```
### Push local changes back upstream:
```bash
# Frontend
git subtree split --prefix=frontend -b frontend-split
git push https://github.com/aydgnme/uniSync.git frontend-split:main
git branch -D frontend-split

# Backend
git subtree split --prefix=backend -b backend-split
git push https://github.com/aydgnme/uniSync-backend.git backend-split:main
git branch -D backend-split
```

## Tech Stack
	•	Mobile: Expo + React Native + TypeScript
	•	API: Fastify + TypeScript
	•	Data: PostgreSQL / Supabase
	•	Tooling: ESLint, TSConfig, Babel; Git subtrees for monorepo management
