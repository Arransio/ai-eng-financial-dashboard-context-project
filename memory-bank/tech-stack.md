# Stack tecnológico

## Frontend

- React 19.2
- TypeScript 6
- Vite 8
- Tailwind CSS 4 mediante plugin de Vite
- Recharts 3
- Vitest y ESLint

Scripts definidos en `frontend/package.json`: `dev`, `build`, `lint`, `test`, `test:watch`, `test:coverage`.

## Backend

- Python 3.13 en el Dockerfile
- FastAPI
- Pydantic
- Uvicorn
- debugpy
- pytest y httpx para pruebas

Dependencias: `backend/requirements.txt`.

## Operación

- Docker Compose define `frontend` y `backend`.
- Frontend publicado en `5173` y backend en `8000`.
- El puerto `5678` pertenece al listener de debug del backend.
- Vite redirige `/api` al servicio Compose `backend:8000`.
- FastAPI expone `/health`, `/docs` y los endpoints `/api/metrics*`.

Evidence: `frontend/package.json`, `backend/Dockerfile`, `backend/requirements.txt`, `docker-compose.yml`, `frontend/vite.config.ts`, `backend/app/main.py`, `backend/app/routes.py`.
