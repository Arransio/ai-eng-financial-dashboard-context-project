# Resumen de proyecto

## Propósito

El repositorio contiene un dashboard financiero para visualizar ingresos, gastos, beneficio y tendencia mensual. La aplicación sirve datos simulados: no hay base de datos ni integración con una fuente financiera real.

## Arquitectura verificada

- Frontend: React 19, TypeScript y Vite en `frontend/`.
- Backend: FastAPI y Pydantic en `backend/app/`.
- Comunicación local: el navegador solicita `/api/metrics`; Vite redirige `/api` al servicio `backend:8000` dentro de Compose.
- Con datos externos alternativos, `VITE_API_BASE_URL` puede cambiar el origen de la API.

## Servicios documentados

- Frontend: puerto publicado 5173.
- Backend: puerto publicado 8000 y puerto debug 5678.
- OpenAPI: `/docs` en el backend.
- Health check: `GET /health`.

## Flujo de datos

1. `frontend/src/App.tsx` solicita `/api/metrics` al cargar.
2. FastAPI genera 360 movimientos simulados con semilla 42.
3. El frontend agrega totales y datos mensuales en `frontend/src/lib/financial-utils.ts`.
4. Los componentes presentan KPIs y gráficos.

## Estado actual

- El frontend tiene pruebas, lint y build comprobables mediante los scripts de `frontend/package.json`.
- El backend tiene pruebas pytest en `backend/tests/test_routes.py`.
- El arranque completo con Docker no pudo comprobarse en esta máquina porque Docker no está instalado.
