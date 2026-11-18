# Validation workflow

Make the smallest targeted change and verify it using the repository's own commands.

- Frontend: run `cd frontend && npm test`, `npm run lint`, and `npm run build` for changed frontend code.
- Backend: run the available pytest suite with `cd backend && pytest` in a Python environment compatible with the Dockerfile (`python:3.13-slim`).
- Do not claim a Docker or HTTP integration check succeeded unless it was actually executed. This workspace currently has no `docker` executable.
- After changes, check `git diff --check` and the exact diff before committing.
- Keep changes focused; do not add visual redesigns or data-product features unless the task explicitly requires them.

Evidence: `frontend/package.json`, `backend/requirements.txt`, `backend/Dockerfile`, `backend/tests/test_routes.py`.
