# Eje Pipelines — Airflow

**Pregunta del eje**: ¿cómo garantizamos que cada corrida ejecuta lo que debe,
cuando debe, y que nada se publica sin pasar los gates?

## Herramienta: Airflow

Épica `qval-95p` (A1–A4 cerradas, A5 abierta). Ver `airflow-local/dags/`.

## DAGs actuales (separación del monolito hecha)

Cada DAG invoca `src/main.py` con `QVAL_TASK=<bloque>` (dispatcher, sin
cambiar lógica). Schedules post-cierre:

| DAG | Bloque | Schedule |
|---|---|---|
| `ust_daily` | treasuries + etf | 21:00 |
| `reference_calc_daily` | characteristics + coupon + money_market | 21:30 |
| `rf_pricing_daily` | pricing RF | 22:00 |
| `curves_daily` | corporate/usd/doplink/aggregate | 22:30 |
| `liquidity_daily` | liquidity + price_inputs + qval_score | 23:00 |
| `vectors_daily` | provisional + final sweep + validation | vie 23:30 |
| `reference_daily` / `publish_olap_daily` | promoción a CH/PG | — |
| `drix_daily` | índices (migrado tal cual del cron) | 17:00/17:05/17:45 |
| `smoke_conns` | conectividad | — |

Guardas: `guard_weekend` (sin sesión RD fines de semana), retries +15min.
`qval-hyb`: escritores migrados a dual-write SQL+CH, corte total pendiente.

## Pendiente (proyecto propuesto)

1. **A5 — congelar el monolito**: apagar el cron tras 1 semana verde. `qval-95p.5`.
2. **Gates en DAGs**: hoy los DAGs calculan y publican; la propuesta es que
   `vectors_daily` (validation) y futuros checks F1/F2 **bloqueen el publish**
   (`QC.PROMOTION_GATE`) en vez de solo reportar.
3. **SLAs y alertas**: `qval-9qd.11` (CloudWatch + `/v1/ops/etl-status` + alarmas)
   está bloqueada — F1 debe proponer monitoreo aunque sea manual primero.
4. **OM en Airflow**: los YAML de ingesta/profile/test de OM se programan como
   DAGs cuando OM-cloud se desbloquee (`RUNBOOK-CLOUD.md` §8).

## Tareas de investigación (F1–F3)

- Mapear dependencias reales entre DAGs: ¿qué pasa si `curves_daily` falla?
  ¿`vectors_daily` lo detecta o publica con curvas viejas?
- Definir SLA por DAG (hora límite, reintentos, escalación).
- Diseñar el gate de publish: qué checks bloquean, cuáles solo alertan,
  quién puede forzar (override con veredicto, como el cockpit).
