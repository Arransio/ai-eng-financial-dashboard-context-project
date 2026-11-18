# Data contracts and mock data

Keep API and UI representations aligned before changing financial fields or categories.

- Mirror changes across Pydantic models in `backend/app/routes.py` and TypeScript types in `frontend/src/lib/financial-types.ts`.
- Keep the existing literal values aligned: operations (`income`, `outcome`), categories (`suppliers`, `sales`, `operational`, `administrative`, `others`) and business types (`B2B`, `B2C`).
- Preserve deterministic mock generation using seed `42` for routes that generate dashboard data; it is required for reproducible results and tests.
- Do not replace the mock provider with database assumptions or unrequested persistence.
- If an API shape changes, add or update the corresponding backend and frontend tests.

Evidence: `backend/app/routes.py`, `frontend/src/lib/financial-types.ts`, `backend/tests/test_routes.py`, `frontend/src/lib/financial-utils.test.ts`.
