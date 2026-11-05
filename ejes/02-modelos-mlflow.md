# Eje Modelos — MLflow

**Pregunta del eje**: ¿cómo sabemos que los modelos detrás de los datos son
válidos, están versionados y se comportan estable en producción?

Marco de referencia: **SR 11-7** (Fed) — inventario completo, validación
independiente, monitoreo continuo, trazabilidad.
Ver `estandares/04-model-governance-sr11-7.md`.

## Herramienta: MLflow

Tracking + registry + serving. Épica `qval-c04` (M1–M4 cerradas, M5 abierta).

## Inventario (lo que ya existe clasificado)

- **A) Entrenados**: anomaly classifier, interpolador de curva, liquidity
  scorer (elasticnet), price engines ajustados.
- **B) Determinísticos**: NS/Svensson params diarios, price inference,
  qval_score config, liquidity metrics, spread, opinion pooling, anchoring,
  DRIX calc, reglas RV, yield trends, mm, doplink, etf, coupon.
- **C) Pipelines como trazabilidad**: parent + nested runs con tags.
  **Regla: los DAGs jamás se registran como modelos.**

Base: `mlruns/` local 19MB (ad-hoc) → tracking server local con backend PG
+ Blob artifacts (M1 hecha). Fichas en `mlflow-local/methodology/*.md`
(10 familias) + scripts `register_*.py`.

## Pendiente (proyecto propuesto)

1. **M5 — backend remoto**: DB `mlflow` en PG único + rol acotado, artifact-root
   en Blob prod, migrar runs. Bloqueado hasta PG único remoto. `qval-c04.5`.
2. **Validación formal por familia** (F4): backtesting, sensibilidad,
   límites conocidos — hoy solo hay validación ad-hoc
   (`qval_ml/docs/reports/intertemporal_coherence_experiment.md`).
3. **Monitoreo**: drift de inputs/outputs, desempeño vs baseline.
4. **Promotion**: Staging → Prod con criterios explícitos (M3 hecha en local,
   falta operar el flujo).

## Tareas de investigación (F4, con adelantos en F0–F2)

- Completar el inventario: ¿hay modelos en `qval_ml` fuera del registro
  (barrido M2-bis ya hecho — verificar)?
- Plantilla de **model card** por familia (propósito, datos, métricas,
  límites, dueño).
- Conectar versión-de-modelo → corrida → dato publicado (linaje con F3).
- Proponer qué significa "promover a Prod" para cada familia determinística.
