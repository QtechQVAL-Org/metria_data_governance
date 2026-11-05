# Estándar 04 — Model governance (SR 11-7)

Marco: Supervisión de la Fed **SR 11-7** (model risk management), el estándar
de la industria financiera. Cuatro pilares:

## 1. Inventario completo

Todo modelo registrado con propósito, dueño, versión y estado.
Nuestro inventario A/B/C (`ejes/02-modelos-mlflow.md`) ya existe — mantenerlo
vivo es la primera obligación.

## 2. Validación independiente

Backtesting, sensibilidad a supuestos, límites conocidos, revisión por alguien
distinto a quien lo construyó. Hoy solo hay validación ad-hoc
(`intertemporal_coherence_experiment.md`). F4 propone validación por familia.

## 3. Monitoreo continuo

Los inputs siguen siendo precisos y completos; el desempeño no se degrada;
los cambios de mercado/producto no invalidan el modelo. Frecuencia acorde a
materialidad (el vector diario merece monitoreo diario).

## 4. Trazabilidad y gobierno

Qué versión del modelo produjo qué dato, quién aprobó cada promoción
Staging → Prod, registro de limitaciones y de respuestas a issues.
Conexión directa con linaje F3 y `QC.PROMOTION_GATE`.

## Para el pasante

SR 11-7 suena bancario y pesado; la versión QVAL es proporcional:
model card por familia + validación documentada + monitoreo del vector diario
+ versiones trazables. Eso ya nos pondría por encima de la mayoría de los
data providers regionales.
