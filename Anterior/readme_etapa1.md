# Proyecto — Riesgo Crediticio en Fintech

**Grupo:** `Luciana Chutte; Martin Orlando` · **Caso 2:** Organización privada (Finanzas / Banca-Fintech)
**Dataset:** `deudores_tarjeta_credito.csv` — 55.000 clientes de tarjeta de crédito

Analizamos el riesgo de incumplimiento de pago (`default_flag`) en una cartera de tarjetas
de crédito, auditando primero la calidad de los datos antes de pensar en modelar. Elegimos
este caso porque el crédito es un problema donde la calidad y el sesgo de los datos impactan
directamente en decisiones sobre personas reales.

## Contexto organizacional

En la cadena de valor, Marketing/Ventas origina los datos del cliente al momento del alta
(edad, ingreso, situación laboral), Riesgo define el límite de crédito, el core bancario
genera el uso de la tarjeta en tiempo real, y Cobranzas registra atrasos y deuda. El score
crediticio viene de un buró externo — por eso es una de las variables con más faltantes.

## Auditoría de calidad — lo más relevante

- **Completitud:** 5 columnas con faltantes (hasta ~13%), ninguna en el target.
- **Consistencia (hallazgo principal):** `ratio_uso_credito` mezcla dos escalas de carga
  (proporción vs. "por mil") — hay que normalizarla antes de cualquier modelo.
- **Faltantes clasificados:** `ingreso_mensual` y `score_crediticio_previo` son **MAR**
  (dependen de `situacion_laboral` y antigüedad del cliente); `atrasos_ultimos_12m` y
  `num_dependientes` son **MCAR** (hipótesis); `antiguedad_laboral_meses` es **MNAR**
  (hipótesis, a validar con negocio).
- **Sesgo:** fuerte desbalance de clases (~3.3% de defaults) — condiciona la métrica a usar.

## KPI traducido

**Morosidad (KPI de negocio)** → **clasificación binaria de default** (problema DS) →
**Recall / AUC-PR** como métrica técnica (por el desbalance, priorizamos detectar los
defaults reales por sobre el accuracy general).

## Ultimos pasos realizados

Con la calidad ya auditada, el próximo paso que realizamos es la normalización de `ratio_uso_credito`, decidimos
la estrategia de imputación por tipo de faltante, y avanzar al modelado + dashboard
(Power BI) de la etapa final.

*Detalle completo del análisis en `EDA_calidad_datos.ipynb`.*
