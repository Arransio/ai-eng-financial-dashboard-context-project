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

## Fase 2 — Hallazgos de ingeniería

- Convenciones extraídas de `frontend/vite.config.ts`, `frontend/package.json`, `backend/app/routes.py`, `frontend/src/lib/financial-types.ts` y las suites de pruebas.
- Riesgos documentados sin convertir preferencias personales en reglas: ports, contratos, semilla mock, separación de stacks y comandos oficiales.
- Corrección aplicada: no se propuso "obligar Swagger" como práctica inventada; se indicó que `/docs` existe porque FastAPI lo genera y el README lo documenta.

## Fase 3 — Reglas probadas

- Se añadieron tres reglas en `.agents/rules/`, todas ancladas a archivos reales del repositorio.
- Prueba pequeña real: se añadió una expectativa de Vitest para el caso mensual sin ingresos.
- Resultado: 6/6 pruebas frontend aprobadas; lint aprobado; build aprobado.
- Corrección observada: no se modificó la implementación porque ya calculaba `profitPercent: 0` cuando no hay ingresos; la prueba confirmó el comportamiento existente.
- La regla de validación no convirtió una limitación del entorno en éxito: Docker y el arranque completo siguen sin ejecutarse.

## Fase 4 — Memory bank

- Se creó `memory-bank/` con descripción de producto, stack tecnológico y estado actual.
- Las afirmaciones fueron contrastadas con `App.tsx`, `routes.py`, `main.py`, `Dockerfile`, `docker-compose.yml`, `package.json`, `vite.config.ts` y las suites de pruebas.
- Se documentó explícitamente que los datos son mock y reproducibles mediante semilla 42.
- Se dejó visible la limitación de Docker/Python y no se presentó como validación exitosa.
- Inconsistencia adicional revisada: el header usa texto fijo `2024 - Full Year`, mientras el backend genera fechas relativas a `date.today()`; se registró como limitación conocida, no como corrección sin requerimiento.
