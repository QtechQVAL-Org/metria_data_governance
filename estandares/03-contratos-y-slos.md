# Estándar 03 — Contratos y SLOs de productos de datos

Un producto de datos sin contrato es una tabla con suerte. El contrato es el
acuerdo formal entre quien produce el dato y quien lo consume.

## SLI → SLO → SLA (el patrón estándar)

- **SLI** (indicador): lo medible. Ej. "horas entre cierre y disponibilidad",
  "% del universo con precio".
- **SLO** (objetivo interno): la meta. Ej. "vector RF disponible H+2,
  cobertura ≥ 99%".
- **SLA** (compromiso con el cliente): el SLO con consecuencias.
  Se define después; F1/F2 trabajan a nivel SLO.

## Contrato mínimo por producto (plantilla)

1. Esquema + tipos (la tabla, sus columnas, sus claves).
2. Reglas de calidad (qué checks de `02-checks-por-dimension.md` aplican).
3. SLOs (frescura + cobertura, con números).
4. Linaje (qué corrida, qué código, qué modelo lo produjo).
5. Política de falla (alertar / bloquear publish / fallback + quién decide).

## Punto de partida

Hay 34 data products declarados en OM (`bootstrap/01_entities.py`) pero sin
contrato: son nombres, no garantías. F3 convierte 2–3 en contratos piloto
(vector RF primero: es el insignia).
