# Estándar 02 — Catálogo de checks por dimensión

Plantilla de check: **dimensión · producto · regla · umbral · severidad ·
qué hace al fallar** (alertar / bloquear / fallback).

## Oportunidad (frescura) — F1

- `freshness_max_date`: `max(valuation_date)` por producto vs último día hábil.
  Umbral: SLO por producto (ej. vector RF H+2). Severidad crítica.
- `freshness_run`: ¿corrió el DAG esperado hoy? (log/schedule vs ejecución).
- `staleness_por_fuente`: feeds sin datos (el cockpit ya lo scorea).

## Completitud (cobertura) — F1

- `coverage_universo`: instrumentos con precio hoy / 1,192 del maestro.
- `coverage_densidad`: filas ≈ calendario × universo (el gate OM
  `density_calendar_universe` 1000–5000 es la versión 1).
- `coverage_nulos`: tasa de nulos en columnas clave por tabla.
- `coverage_sin_precio`: lista de ISINs del maestro sin precio hoy (accionable).

## Consistencia — F2

- `xcheck_vector_vs_trades`: publicado vs último trade / VWAP 20d (σ=2.4 pts
  calibrado; ver cockpit).
- `xcheck_dia_vs_dia`: saltos `|Δ|` con z-score (ventana 60 obs) — lo absurdo
  overnight es casi siempre error, no mercado.
- `xcheck_curva_vs_operado`: YTM implícita de trades vs curva (▲ del cockpit).
- `xcheck_residuales`: q-q de residuales NS/Svensson; colas = novedades del día.
- `xcheck_ch_vs_sql`: paridad durante el dual-write (hasta el corte total).

## Validez y unicidad — F2

- Rangos: precio > 0, ytm −0.05…0.5, scores, returns −1…1, weights 0…1.
- Enums: `currency` ISO-3, `quote_side` (BID/ASK/MID), `market` (BVRD/OTC).
- PKs not-null + unicidad (valuation_date, isin) por producto.
- `columnCount` = detector de DDL drift (ya en OM full-coverage).

## Reconciliación externa — F2 (frente más cercano al cliente)

- Vector vs lo tradeado en BVRD con tolerancia: es la definición operativa
  de exactitud para un data provider. Calibrar la tolerancia con historia,
  no adivinarla.

## Deriva (avanzado) — F4

- Distribución de inputs/outputs vs ventana baseline; dispara revisión del
  modelo, no del dato del día.

## Origen

Los checks de validez/unicidad y pisos de filas ya existen como test suites OM
(`openmetadata-local/bootstrap/03_quality_full.py`). F1/F2 agregan frescura,
cobertura vs maestro, consistencia y reconciliación — los huecos reales.
Diseñar en SQL primero, migrar a OM al desbloquearse el cloud.
