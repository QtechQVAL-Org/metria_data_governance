# metria_data_governance — Gobernanza de Datos y Modelos QVAL

**Vendemos datos. La gobernanza no es overhead: es el producto.**

QVAL vende productos de datos (vector de precios RF, curvas, scores, índices DRiX,
renta variable). Cada incidencia de calidad — un dataset desactualizado, una
cobertura incompleta, un vector que no se parece a lo tradeado — no es un bug
interno: es un defecto del producto que llega al cliente. Este repo define la
visión, los estándares y el camino para pasar de *"un ETL que calcula"* a
*"una plataforma que garantiza"*.

## Cómo leer este repo

| Carpeta | Qué es | Para quién |
|---|---|---|
| `estado-actual/` | Qué existe hoy en 1 página por pieza | Punto de partida: léelo primero |
| `estandares/` | El vocabulario y el catálogo de checks | Referencia al proponer validaciones |
| `ejes/` | Los 3 proyectos de gobernanza y sus herramientas | Dónde vive cada iniciativa |
| `roadmap.md` | Fases como preguntas de investigación | Tu plan de trabajo |
| `historico/` | Anexo de incidencias pasadas (evidencia, no tareas) | Motivación: por qué importa esto |
| `biblio/` | Fuentes: DAMA, SR 11-7, data contracts, docs de herramientas | Profundizar |

## Los 3 ejes

1. **Datos** → OpenMetadata: catálogo, calidad, contratos y SLOs por producto.
   Ver `ejes/01-data-openmetadata.md`.
2. **Modelos** → MLflow: inventario, validación, registry y monitoreo.
   Ver `ejes/02-modelos-mlflow.md`.
3. **Pipelines** → Airflow: DAGs por dominio, SLAs y gates que bloquean el publish.
   Ver `ejes/03-pipelines-airflow.md`.

## El estado en una frase por pieza

- `qval_ml` calcula (pricing, curvas, scores, DRiX) pero **no tiene gates**: si corre, publica.
- Lo servido vive en **ClickHouse**; SQL Server es legacy en retirada.
- Existe un **validation cockpit** (front + back + ETL) medio dormido: es la semilla
  del eje de validación continua. Ver `estado-actual/03-validation-cockpit.md`.
- OpenMetadata, MLflow y Airflow están **sembrados en local**, pendientes de llevar a cloud.

## Regla de oro

> Todo check que propongas debe mapear a una dimensión de `estandares/01-dimensiones-dama.md`,
> pertenecer a un producto con SLO (`estandares/03-contratos-y-slos.md`),
> y terminar viviendo en uno de los 3 ejes — nunca como script suelto.
