# Eje Datos — OpenMetadata

**Pregunta del eje**: ¿cómo sabemos que cada producto de datos cumple su contrato,
todos los días, antes de publicarse?

## Herramienta: OpenMetadata (Collate)

Catálogo + ingesta de metadata + profiler + test suites + linaje, open-source.
Un service `clickhouse_qval` apunta al warehouse servido; los tests se crean por
API y se ejecutan por YAML (programables como DAGs de Airflow).

## Lo sembrado (local, 2026-10-10)

- 6 dominios: RentaFija, Curvas, RentaVariable, DRIX, ControlCalidad, Referencia.
- 34 data products (1 por tabla final) + glosario `qval-es.csv` (~150 términos).
- Gate EPIC-11: 10 test cases (not-nulls en PKs, `clean_price > 0`,
  densidad calendario-universo `rowCount 1000–5000`).
- Cobertura full 37 tablas: not-null en claves, rangos por tipo
  (precios > 0, ytm −0.05…0.5, returns −1…1), sets (`currency`, `quote_side`,
  `market`), piso de filas por tabla, `columnCount` (detecta DDL drift).
- Validado: 37 tablas metadata 100%, profiler 100%, quality 100%.

Ver `openmetadata-local/`: `bootstrap/01_entities.py`, `02_quality.py`,
`03_quality_full.py`, `ingestion/quality/*.yaml`, `RUNBOOK-CLOUD.md`.

## Pendiente (proyecto propuesto)

1. **Desbloquear cloud**: ES Serverless rechaza settings de índices OM (issue #26441).
   Caminos: Elastic Hosted o esperar 2.0.6+. `qval-9qd.9`.
2. **Usage/linaje**: pendiente por bug del source clickhouse-usage 2.0.5;
   workaround interino = linaje declarado vía `qc_calc_runs`. Reintentar en 2.0.6+.
3. **Glosario**: bulk upload del CSV una vez validado el catálogo.
4. **Contratos**: pasar de test cases sueltos a contrato versionado por producto
   (esquema + reglas + SLO) — es F3 del roadmap.

## Tareas de investigación (F1–F3)

- Mapear cada test existente a una dimensión DAMA (`estandares/01-dimensiones-dama.md`).
- Identificar qué frentes NO cubre OM hoy: frescura explícita (`max(fecha)` vs hoy),
  cobertura vs maestro, coherencia cruzada, reconciliación vs tradeado.
- Proponer el catálogo F1/F2 en SQL primero (no bloqueado por el cloud),
  diseñado para migrar a test suites OM cuando se desbloquee.
