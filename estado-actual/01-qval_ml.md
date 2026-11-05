# Estado actual 01 — `qval_ml`: el monolito que calcula

`qval_ml/` es el cerebro de cálculo de la plataforma. Todo lo que se publica
pasa por aquí. Hoy **calcula sin gates**: si el código corre, el dato se publica.

## Qué calcula (bloques de `src/main.py`, vía `QVAL_TASK`)

- **pricing**: vector RF (precios limpio/sucio, YTM, duración).
- **curves**: Nelson-Siegel/Svensson corporativas, USD, DOPLINK, agregado.
- **ust**: treasuries + ETF-linked.
- **liquidity**: liquidity scores (1–6), price inputs, qval_score.
- **reference**: characteristics, coupon schedule, money market.
- **vectors**: sweep provisional + final + validation.
- **DRIX** (`src/indexlib/`): índices base 100, pesos, retornos, contribuciones.

## Escritura

Dual-write **SQL Server + ClickHouse** (`qval-hyb`). SQL es fuente de lectura
legacy y sistema de record transitorio; CH (`qval_dev`) es lo servido.
Corte total a CH pendiente.

## Lo que NO tiene (y por eso existe este repo)

- Ningún bloque valida antes de publicar.
- Ningún bloque declara su contrato (qué garantiza) ni su SLO (cuándo).
- La validación vive en un job lateral (`validation/`), no en el camino del publish.

## Para el pasante

No necesitas entender cada modelo en F0. Necesitas el mapa: qué bloque produce
qué tabla, en qué orden corren, y dónde (en ese flujo) debería vivir cada gate.
Empieza por `qval_ml/src/etl/` (los jobs) y `airflow-local/dags/etl_blocks.py`
(el orden).
