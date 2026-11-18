# Rastro de verificación

## Fase 1 — Comprensión del handover

### Resumen verificado

| Afirmación | Estado | Evidencia |
|---|---|---|
| El producto es un dashboard financiero con frontend React + TypeScript y backend FastAPI | ✅ Verificada | `README.md`; `frontend/package.json`; `backend/app/main.py` |
| Docker Compose levanta `frontend` y `backend` | ✅ Verificada | `docker-compose.yml` |
| El frontend se sirve en el puerto 5173 | ✅ Verificada | `docker-compose.yml`; `frontend/Dockerfile`; `README.md` |
| El backend expuesto por la API usa el puerto 8000 | ✅ Verificada | `docker-compose.yml`; `backend/Dockerfile`; `README.md` |
| La documentación OpenAPI está en `/docs` | ✅ Verificada | `backend/app/main.py` (FastAPI crea `/docs`); `README.md` |
| En Docker, el frontend reenvía `/api` al backend mediante el proxy de Vite | ✅ Verificada | `frontend/vite.config.ts` (`http://backend:8000`) |
| El backend genera datos financieros aleatorios deterministas con semilla 42 | ✅ Verificada | `backend/app/routes.py` (`generate_mock_movements(seed=42)`) |
| La UI calcula KPIs y datos mensuales a partir de `/api/metrics` | ✅ Verificada | `frontend/src/App.tsx`; `frontend/src/lib/financial-utils.ts` |
| Los datos cubren 360 movimientos de un año | ✅ Verificada | `backend/app/routes.py` (`12` meses × `30` movimientos); `backend/tests/test_routes.py` |

### Correcciones durante la verificación

- **❌ Supuesto descartado:** ejecutar el proyecto con Docker era posible en este entorno. **Corrección:** el ejecutable `docker` no está instalado y `docker compose` no está disponible. No se modificó el proyecto; queda documentada la limitación del entorno.
- **❌ Supuesto descartado:** el sistema Python actual puede ejecutar el backend. **Corrección:** el entorno reporta Python 3.7.7, mientras el Dockerfile fija Python 3.13 y el código usa tipos modernos compatibles con versiones posteriores. No se inició una sustitución sin evidencia.
- **❓ Sin ejecutar:** arranque completo con Compose y pruebas HTTP. Queda sin verificar hasta disponer de Docker o de un runtime Python compatible.

### Validaciones ejecutadas

- `npm install`
- `npm test`: 5 pruebas aprobadas
- `npm run lint`: aprobado
- `npm run build`: aprobado
- Advertencia observada: el build informa un bundle mayor de 500 kB; no se realizó un refactor visual por no ser necesario para esta tarea.
