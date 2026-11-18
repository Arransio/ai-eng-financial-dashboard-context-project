# Repository architecture

Before editing, identify whether the change belongs to `frontend/` or `backend/`.

- Keep React/Vite code under `frontend/` and FastAPI/Pydantic code under `backend/`.
- For Docker Compose changes, preserve the service names `frontend` and `backend` because `frontend/vite.config.ts` proxies `/api` to `http://backend:8000`.
- Preserve the documented ports unless Compose and Dockerfiles are updated together: frontend `5173`, backend `8000`, debug listener `5678`.
- Do not treat `localhost` or a fixed external backend URL as established inside Compose: use the `/api` proxy as the default.

Evidence: `docker-compose.yml`, `frontend/vite.config.ts`, `frontend/Dockerfile`, `backend/Dockerfile`.
