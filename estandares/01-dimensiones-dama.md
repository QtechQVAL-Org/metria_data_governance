# Estándar 01 — Dimensiones de calidad (DAMA-DMBOK)

El vocabulario oficial del repo. Todo check propuesto debe nombrar su dimensión.
Fuente: DAMA International, DMBOK.

| # | Dimensión | Pregunta | Ejemplo QVAL |
|---|---|---|---|
| 1 | **Exactitud** | ¿El valor refleja la realidad? | precio publicado ≈ precio tradeado (tolerancia) |
| 2 | **Completitud** | ¿Falta algo? (tasa de llenado) | % del universo 1,192 con precio hoy; nulos |
| 3 | **Consistencia** | ¿Coincide entre sistemas/tiempos? | vector vs last_trade; hoy vs ayer; CH vs SQL |
| 4 | **Oportunidad** | ¿Está al día? (frescura) | `max(valuation_date)` vs calendario hábil; lag en horas |
| 5 | **Unicidad** | ¿Hay duplicados? | (valuation_date, isin) única en price_vector |
| 6 | **Validez** | ¿Cumple formato/reglas? | precio > 0, ytm −5%…50%, enums (`BID/ASK/MID`), PKs not-null |

Regla: si un check no mapea a una de estas seis, probablemente está mal definido.
Ver `02-checks-por-dimension.md` para el catálogo.
