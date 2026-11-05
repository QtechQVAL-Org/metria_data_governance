# Biblio — fuentes del repo

## Marcos y estándares

- **DAMA-DMBOK** — 6 dimensiones de calidad (exactitud, completitud, consistencia,
  oportunidad, unicidad, validez). https://www.dama.org/dama-dmbok-revision/
- **SR 11-7 (Federal Reserve)** — model risk management: inventario, validación,
  monitoreo, trazabilidad.
  https://www.federalreserve.gov/boarddocs/srletters/2011/sr1107.pdf
- **Data contracts / data mesh** — SLI → SLO → SLA, domain ownership.
  https://www.datamesh-governance.com/policies/definitions/data-contract/data-contract

## Herramientas (docs locales en el monorepo)

- OpenMetadata: `openmetadata-local/` (bootstrap, quality YAMLs, `RUNBOOK-CLOUD.md`)
- MLflow: `mlflow-local/methodology/*.md` + `qval_ml/docs/mlflow_docs/local_serving.md`
- Airflow: `airflow-local/dags/`

## Antecedentes internos

- Validation cockpit: `qval_ml/src/qval_ml/validation/metrics.py`,
  `qtechqvalback/app/routers/validacion.py`,
  `qtechqvalfront/src/app/(app)/validacion/page.tsx`
- Coherencia intertemporal: `qval_ml/docs/reports/intertemporal_coherence_experiment.md`
- Decisiones: memorias beads `pg-ch-pivot-decision`, `sql-ch-gap`, `etl-ch-sole-write-target`
- Metodología: `metria_docs/` (repo hermano — el sistema de manuales del regulador)
