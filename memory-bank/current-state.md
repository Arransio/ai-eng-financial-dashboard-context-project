# Estado actual

## Implementado

- Aplicación React que consulta el backend al iniciar.
- Cálculo de KPI y series mensuales en el frontend.
- API FastAPI con filtros, facets, resúmenes, categorías, comparación y alertas.
- Datos mock deterministas con semilla `42`.
- Pruebas backend existentes y pruebas frontend existentes.
- Reglas de agente en `.agents/rules/`.

## Validado en esta tarea

- `npm test`: 5 pruebas aprobadas al inicio; 6 aprobadas tras añadir el caso mensual sin ingresos.
- `npm run lint`: aprobado.
- `npm run build`: aprobado con una advertencia de tamaño de bundle superior a 500 kB.
- `git diff --check`: aprobado antes de los commits por fase.

## No validado

- `docker compose up --build`: no disponible porque Docker no está instalado en este entorno.
- Pruebas HTTP completas y arranque integrado: no ejecutados por la ausencia de Docker y por tener Python 3.7.7 local frente al Python 3.13 del Dockerfile.
- Backend pytest: no se inició con una sustitución no compatible; la infraestructura prevista está declarada en `backend/Dockerfile` y `backend/requirements.txt`.

## Limitaciones conocidas

- Los datos son simulados y cambian los periodos según `date.today()` dentro de la generación anual.
- El header puede presentar explícitamente `2024 - Full Year` aunque `App.tsx` consume datos generados desde el año relativo actual en `routes.py`; esto debe revisarse antes de asumir consistencia de periodos.
- El backend permite CORS amplio (`allow_origins=["*"]`) y no usa persistencia.

Evidence: `frontend/src/App.tsx`, `backend/app/routes.py`, `backend/app/main.py`, `docker-compose.yml`, `verification.md`.
