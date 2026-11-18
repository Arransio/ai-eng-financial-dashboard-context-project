# Hallazgos de ingeniería derivados del repositorio

## Convenciones existentes

1. **Servicios separados por directorio**: `frontend/` contiene React/Vite y `backend/` contiene FastAPI. No mezclar dependencias ni código de servicio.
2. **Alias `@` en el frontend**: `frontend/vite.config.ts` configura `@` como `frontend/src`. Las importaciones internas existentes lo usan en `App.tsx`.
3. **Tipos financieros compartidos conceptualmente**: el backend define literales Pydantic y el frontend mantiene interfaces equivalentes en `frontend/src/lib/financial-types.ts`. Las modificaciones de categoría, operación o modelo deben mantenerse alineadas.
4. **Datos simulados reproducibles**: todas las rutas principales usan `generate_mock_movements(seed=42)`. Cambiar la semilla o eliminarla alterará resultados y pruebas.
5. **Agregaciones separadas**: el backend contiene filtros y resúmenes en `backend/app/routes.py`; el frontend agrega KPIs y series mensuales en `frontend/src/lib/financial-utils.ts`. Evitar duplicar lógica sin una necesidad documentada.
6. **Pruebas junto al dominio**: Python tiene pruebas en `backend/tests/test_routes.py`; Vitest vive junto a utilidades como `frontend/src/lib/financial-utils.test.ts`.
7. **Scripts oficiales en cada stack**: frontend usa los scripts npm de `frontend/package.json`; backend usa pytest e las dependencias de `backend/requirements.txt`.

## Riesgos para futuras ediciones

- Cambiar puertos o nombres de servicio sin actualizar Compose y el proxy puede romper la comunicación.
- Modificar nombres de campos en `FinancialMovement` sin actualizar Pydantic, TypeScript y transformaciones puede romper fetch, renderizado y pruebas.
- Añadir categorías u operaciones sin actualizar literales en ambos stacks puede causar tipos válidos en un lado y rechazados en el otro.
- Retirar la semilla 42 hace menos reproducibles los datos, filtros y resultados visuales.
- Mover utilidades fuera del directorio `lib` o cambiar el alias sin coordinación rompe importaciones existentes.
- Ejecutar el backend fuera de Python 3.13 puede ser incompatible; el Dockerfile es la evidencia principal de la versión soportada.
- Reducir o eliminar health/OpenAPI sin actualizar documentación y operación deja señales de arranque desalineadas.

## Reglas propuestas

| Regla | Hecho concreto del repositorio |
|---|---|
| Mantener separación frontend/backend | Directorios y Dockerfiles independientes; Compose depende de dos servicios |
| Validar cambios mediante pruebas y scripts existentes | `package.json`, `test_routes.py` y `financial-utils.test.ts` |
| Mantener contratos de datos alineados | Literales Pydantic en `routes.py` e interfaces TypeScript en `financial-types.ts` |
| Preservar reproductibilidad de datos mock | Semilla 42 en todas las rutas que generan movimientos |
| Usar el proxy `/api` y el nombre `backend` en Compose | `vite.config.ts` apunta a `http://backend:8000` |
| No asumir puertos sin Compose/Dockerfiles | 5173 y 8000 están publicados; 5678 es el puerto de debug |
| Documentar afirmaciones con rutas y verificar | El repositorio no tenía memoria ni reglas antes de esta tarea |

## Verificación pendiente

- Docker Compose no pudo ejecutarse en el entorno actual porque el binario Docker no está disponible.
- Las pruebas frontend sí pasaron; backend e integración HTTP siguen pendientes hasta tener Docker o Python 3.13 compatible.
