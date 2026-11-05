# Histórico de incidencias (anexo — evidencia, no tareas)

> Estas fichas existen para responder *"¿por esto importa la gobernanza?"*.
> No son trabajo asignado: son la motivación del roadmap.
> Formato: fecha · producto · frente · causa · fix · bead · **qué check lo habría atrapado**.

## 2026-10-13 — FX.VARIABLE_FACT congelado (57/63 series muertas)

- **Producto**: `variable_fact` (FX). **Frentes**: datasets desactualizados, ETL no ejecutado, frescura.
- **Causa**: loader del ETL .8 caído desde 10-13; solo 6 series dólar seguían vivas.
- **Fix**: pendiente — ubicar el loader o absorber la carga (fuentes BCRD + encuestas). `qval-fz0` (open P1).
- **Lo habría atrapado**: `freshness_max_date` por serie + `coverage_universo` (F1).

## 2026-10-07/22 — EQLINK-SPY sin filas PV dos semanas

- **Producto**: `price_vector` (ETF-linked). **Frente**: cobertura.
- **Causa**: el cálculo solo corría "hoy", sin backfill del rango 10-07…10-22.
- **Fix**: pendiente — backfill del rango. `qval-1c2` (open P3).
- **Lo habría atrapado**: `coverage_densidad` calendario × universo (F1).

## 2026-10-23 — external_closes: corrida que falla por quoting

- **Producto**: `EXTERNAL_CLOSES`. **Frente**: ETL/corrida fallando.
- **Causa**: INSERT sin quoting de columnas `OPEN`/`CLOSE` (reservadas MSSQL).
- **Fix**: pendiente — ORM/bindparams con quoting. `qval-drx` (open P2).
- **Lo habría atrapado**: gate de schema/contrato en el DAG (F3) + test de integración.

## 2026-10-26 — Impugnaciones: resolución escribe al lugar equivocado

- **Producto**: correcciones del vector. **Frente**: calidad (correcciones que no llegan).
- **Causa**: la resolución escribe a `PRICE_VECTOR` por conexión PG, pero el vector
  vive en CH; además SQL de SQL Server en código PG. `qval-er2` (open P0).
- **Lo habría atrapado**: contrato con linaje explícito de escritura (F3).

## 2026-11-02 — Rezago SQL vs CH de 3 días + tablas huérfanas

- **Producto**: varios (`price_vector`, `price_score`, `signal_trade`…).
- **Frentes**: datasets desactualizados, ETL no publicado.
- **Causa**: el ETL escribía SQL a diario pero nadie publicaba a CH desde ~10-27;
  `PRICE_INPUTS` solo existía en SQL, `price_vector_history` vacía en CH.
- **Fix**: dual-write directo a CH (piloto validado, corte total pendiente). `qval-hyb`.
- **Lo habría atrapado**: `xcheck_ch_vs_sql` paridad + `freshness_max_date` por destino (F1/F2).

## Estructural — Sin observabilidad del ETL

- Sin `/v1/ops/etl-status` ni alarmas: nadie se entera cuando una corrida no corre.
  `qval-9qd.11` (open, bloqueada). **Es el detector que F1 debe proponer.**

## Estructural — Suite de tests rota

- 19 + 13 fallos en la suite base (fixtures sin HISTORY, firmas desactualizadas).
  `qval-2a8`, `qval-e0m`. La red de seguridad interna también necesita dueño.
