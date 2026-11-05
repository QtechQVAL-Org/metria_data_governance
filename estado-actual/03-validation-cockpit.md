# Estado actual 03 — Validation Cockpit: la semilla a rescatar

Existe un **cockpit de validación por corrida** casi completo, hoy medio dormido.
No hay que construir validación de cero: hay que **auditarla, reactivarla y
convertirla en gate**. Es tu primera tarea real (F0/F2).

## Qué es (plan v11)

Un flujo diario: el ETL genera un reporte por fecha de vector → el analista lo
revisa → emite veredicto (aprobar/rechazar) → si no actúa antes del deadline,
**auto-valida** (el vector oficial nunca se atrasa). Las impugnaciones abiertas
**bloquean** la aprobación.

## Las 3 capas (todas existen en el repo)

**ETL** — `qval_ml/src/qval_ml/validation/metrics.py` (+ `src/etl/validation_report.py`):
severity por instrumento combinando diff vs último trade (σ=2.4 pts calibrado en
datos reales 2026-08), VWAP 20d, z-score de cambio diario (ventana 60 obs),
residuales NS/Svensson en bps, decaimiento de frescura exponencial (half-life 30d),
detección de suelos de estructurados. Umbrales: FLAG 25, CRITICAL 60 (alerta email).

**Back** — `qtechqvalback/app/routers/validacion.py` (solo analistas):
sirve `QC.VALIDATION_REPORTS / VALIDATION_ITEMS`, registra veredictos en
`QC.PROMOTION_GATE` (último veredicto gana).

**Front** — `qtechqvalfront/src/app/(app)/validacion/page.tsx` ("cockpit del analista"):
cola con countdown, scorecard (instrumentos, flaggeados, severidad máx, fallback,
feeds sin datos), **q-q plot de residuales** (colas = novedades del día),
ranking por severidad con drill-down (curva hoy/−1sem/−1mes, tasa 30d vs YTM
implícita de trades ▲, trades que entraron al modelo, informe PDF paso a paso).

## Estado real y preguntas a auditar

- La entrada del nav es **condicional por rol** (403 silencioso → no aparece):
  parece deshabilitada, pero el código sigue ahí. Ver `qtechqvalfront/lib/nav.ts`.
- ❓ ¿El ETL sigue escribiendo `QC.VALIDATION_REPORTS` cada día?
- ❓ ¿Qué fechas tienen reporte y cuáles no? ¿Los veredictos se registran?
- ❓ ¿La severidad sigue calibrada (σ=2.4 con datos actuales)?

## Por qué importa para el roadmap

El cockpit es el prototipo de todo lo que F2/F3 quieren generalizar:
score por instrumento, umbrales, veredicto humano + auto-aprobación,
y `PROMOTION_GATE` como el punto donde un gate **bloquea el publish**.
Tu propuesta F2 debe partir de aquí, no de una hoja en blanco.
