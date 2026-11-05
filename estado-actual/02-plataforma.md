# Estado actual 02 — Plataforma: lo servido y los productos

## Dónde viven los datos

- **ClickHouse** (`qval_dev`, hoy self-hosted AWS): lo que sirve el back y el front.
  ~37 tablas finales.
- **SQL Server** (`DEVELOPMENT.QVAL`): legacy en retirada — aún fuente de lectura
  para algunas series y de escritura dual transitoria.
- **Postgres** (plataforma/OLTP): usuarios, API keys, impugnaciones, veredictos —
  NO datos de mercado.

## Los productos (dominios OM, 1 tabla final = 1 producto)

- **RentaFija**: `price_vector` (el producto insignia), `price_score`,
  `liquidity_score`, `last_trade_mart`, `trading_volume_mart`.
- **Curvas**: `yield_curve`, `money_market_curve`, `curve_params`.
- **RentaVariable**: `equity_price_vector`, `equity_screener`, NAVs, flujos, eventos.
- **DRIX**: `index_daily_levels_fact`, AGGs, dims compactadas.
- **ControlCalidad** (interno): `qc_calc_runs`, validation reports, challenges.
- **Referencia** (interno): maestro, moneda, FX, CPI.

Universo maestro: `qval_ml/maestro_titulos_updated.csv`, **1,192 títulos**.
Es la referencia contra la que se mide cobertura.

## Para el pasante

Cada producto necesita, eventualmente: dueño, SLO (hora + cobertura),
contrato (esquema + reglas) y gate. F1 empieza definiendo los SLOs;
no necesitas tocar la plataforma, solo leerla.
