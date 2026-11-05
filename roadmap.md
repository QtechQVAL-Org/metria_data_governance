# Roadmap — de ETL que calcula a plataforma que garantiza

Cada fase es una **pregunta de investigación**, no una lista de fixes.
Cada fase entrega un documento de propuesta + (cuando aplique) un prototipo.
Nada se implementa en producción sin pasar por los gates de la plataforma.

## F0 — Entender (semanas 1–2)

> *¿Qué calcula la plataforma, qué valida hoy, y dónde están los huecos?*

- Recorrer `estado-actual/`: `qval_ml`, plataforma CH, validation cockpit.
- Auditar el cockpit: ¿el ETL sigue alimentando `QC.VALIDATION_REPORTS`?
  ¿Los reportes llegan? ¿Qué fechas quedaron sin reporte?
- Correr a mano 5 queries: `max(valuation_date)` por producto, conteo de
  instrumentos vs maestro, nulos en columnas clave.
- **Entrega**: nota de 2 páginas — qué funciona, qué está dormido, qué falta.

## F1 — Frescura y cobertura (semanas 3–5)

> *¿Cómo sabemos, cada día, que cada producto está al día y completo?*

- Definir **SLO por producto**: hora límite + cobertura mínima vs universo maestro
  (1,192 títulos). Ver `estandares/03-contratos-y-slos.md`.
- Proponer checks SQL de frescura (`max(fecha)` vs calendario hábil) y cobertura
  (densidad calendario × universo, instrumentos sin precio hoy).
- Proponer **dónde vive el gate**: ¿qué pasa cuando falla? (alerta, bloqueo, fallback).
- **Entrega**: catálogo F1 + SLOs propuestos por producto.

## F2 — Consistencia y validez (semanas 6–9)

> *¿Cómo sabemos que lo publicado es correcto, no solo que existe?*

- Coherencia cruzada (vector vs `last_trade`, curvas vs operado) y día-vs-día
  (saltos `|Δ|` con tolerancia calibrada).
- Reconciliación vs lo tradeado (BVRD) con tolerancia — el frente más cercano al cliente.
- Rescatar y extender la metodología del validation cockpit (severity, z-scores,
  q-q de residuales) como base, no empezar de cero.
- **Entrega**: catálogo F2 + propuesta de reactivación del cockpit como gate.

## F3 — Contratos y linaje (semanas 10–14)

> *¿Cómo pasamos del check suelto al contrato versionado y trazable?*

- Del check SQL al **contrato por producto** (esquema + reglas + SLO).
- Linaje corrida → código → modelo → dato (`qc_calc_runs` + MLflow).
- Aquí entra **OpenMetadata en cloud** (desbloquear ES/ingesta) como el hogar
  de catálogo + quality + linaje. Ver `ejes/01-data-openmetadata.md`.
- **Entrega**: 2–3 contratos piloto + propuesta de despliegue OM.

## F4 — Modelos (semanas 15+)

> *¿Cómo garantizamos que los modelos detrás de los datos son válidos y estables?*

- Model cards, validación independiente, promoción Staging → Prod, monitoreo
  (drift de inputs/outputs, desempeño).
- Aquí entra **MLflow remoto (M5)** y el registry como fuente de verdad de
  versiones. Ver `ejes/02-modelos-mlflow.md`.
- **Entrega**: plantilla de model card + plan de validación por familia de modelos.

## Dependencias conocidas (no te bloquean)

- OM-cloud postergado por incompatibilidad ES Serverless → F1/F2 se hacen en SQL,
  F3 lo desbloquea. `qval-9qd.9`.
- MLflow remoto (M5) bloqueado hasta PG único → F4. `qval-c04.5`.
- Corte total SQL → CH pendiente → el linaje debe contemplar ambas escrituras. `qval-hyb`.
- Observabilidad (`/v1/ops/etl-status` + alarmas) bloqueada → F1 debe proponer
  el monitoreo aunque sea manual primero. `qval-9qd.11`.
