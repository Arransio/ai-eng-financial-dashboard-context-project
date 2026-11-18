# Descripción de producto

El proyecto es un dashboard financiero que presenta una vista anual de métricas de una empresa ficticia.

## Usuarios y objetivo

- Usuario: persona que necesita revisar ingresos, gastos y beneficio.
- Objetivo: mostrar KPI principales y una comparación mensual de ingresos frente a gastos.
- Alcance: visualización local de datos simulados; no incluye autenticación, persistencia ni integración con sistemas contables.

## Funcionalidad visible

- KPIs de ingreso total, gasto total, beneficio y porcentaje de beneficio.
- Gráfico de ingresos frente a gastos por mes.
- Gráfico de porcentaje de beneficio por mes.
- Estado de carga y estado de error si la API no responde.

## Datos

El backend genera 360 movimientos: 30 por mes durante 12 meses, con semilla `42` para hacerlo reproducible. Los movimientos tienen fecha, importe, tipo de operación, categoría y tipo de negocio.

Evidence: `frontend/src/App.tsx`, `frontend/src/lib/financial-utils.ts`, `backend/app/routes.py`, `backend/tests/test_routes.py`.
