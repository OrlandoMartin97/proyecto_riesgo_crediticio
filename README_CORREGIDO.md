# Proyecto — Riesgo Crediticio en Fintech

**Grupo:** Luciana Chutte; Martin Orlando  
**Caso 2:** Organización privada — Finanzas / Banca-Fintech  
**Dataset:** `deudores_tarjeta_credito.csv` — 55.000 clientes de tarjeta de crédito

## Descripción del proyecto

Analizamos el riesgo de incumplimiento de pago (`default_flag`) en una cartera de tarjetas de crédito. El trabajo parte de una auditoría de calidad de datos, continúa con la limpieza y normalización del dataset y utiliza un dashboard en Power BI para comunicar los principales indicadores.

Elegimos este caso porque el riesgo crediticio es un problema de negocio donde la calidad de los datos y los posibles sesgos pueden impactar directamente en decisiones sobre clientes.

## Preguntas de negocio

El análisis y el dashboard buscan responder principalmente estas tres preguntas:

1. **¿Qué características presentan los clientes con mayor tasa de incumplimiento?**  
   Se comparan variables como situación laboral, nivel educativo, antigüedad como cliente, atrasos previos y nivel de endeudamiento.

2. **¿Cómo se relacionan el uso del crédito, la deuda y los atrasos previos con el riesgo de default?**  
   El objetivo es identificar señales de comportamiento financiero asociadas al incumplimiento que puedan servir para el seguimiento de la cartera.

3. **¿Existen diferencias relevantes en la tasa de default entre distintos segmentos de clientes?**  
   Esta comparación permite detectar concentraciones de riesgo y también revisar posibles sesgos antes de utilizar variables de segmentación en decisiones automatizadas.

Estas preguntas funcionan como hilo conductor del proyecto: el EDA explora si los datos permiten responderlas, el ETL prepara una base consistente y el dashboard presenta los resultados de forma entendible para negocio.

## Contexto organizacional

Dentro de una fintech o entidad financiera, distintas áreas participan en la generación de los datos. Marketing/Ventas registra información del cliente al momento del alta; Riesgo interviene en la definición del límite de crédito; los sistemas transaccionales registran el uso de la tarjeta; y Cobranzas genera información relacionada con atrasos y deuda.

Algunos campos, como `score_crediticio_previo`, podrían provenir de una fuente externa. Como el dataset no incluye metadatos sobre su origen, esta procedencia debe considerarse una hipótesis de negocio y validarse antes de utilizarla como certeza.

## Auditoría de calidad — principales hallazgos

- **Completitud:** cinco columnas presentan valores faltantes; ninguna corresponde a `default_flag`.
- **Unicidad:** no se detectaron filas duplicadas y `id_cliente` es único.
- **Consistencia:** `ratio_uso_credito` mezcla escalas diferentes, por lo que se genera `ratio_uso_credito_normalizado`.
- **Validez:** las variables revisadas respetan los dominios y rangos esperados.
- **Exactitud:** se detectaron valores extremos y algunos casos que requieren validación semántica con negocio, pero no se eliminaron automáticamente.
- **Actualidad:** el dataset no contiene fecha de corte o extracción, por lo que no puede evaluarse su vigencia temporal.
- **Faltantes:** se plantean hipótesis MAR/MCAR/MNAR según cada variable; las que no pueden confirmarse únicamente con los datos quedan documentadas como tales.
- **Sesgo:** `default_flag` está fuertemente desbalanceado; la tasa global de default es aproximadamente **3.32%**, por lo que una futura evaluación de modelos no debería basarse solamente en accuracy.

## KPI de negocio y traducción a Data Science

**KPI de negocio:** tasa de morosidad / default.  
**Problema de Data Science:** clasificación binaria (`default_flag = 1` frente a `0`).  
**Métricas técnicas sugeridas para una etapa de modelado:** Recall y AUC-PR, debido al desbalance de clases.

En esta entrega el foco está puesto en comprender el problema, auditar los datos y preparar una base confiable; no en optimizar un modelo predictivo.

## Archivos del proyecto

- `EDA_calidad_datos.ipynb`: exploración inicial, auditoría de las seis dimensiones de calidad, faltantes, sesgos y preguntas de negocio.
- `ETL_riesgo_crediticio.ipynb`: limpieza, normalización, imputaciones y validaciones posteriores.
- `deudores_tarjeta_credito.csv`: dataset original.
- `deudores_tarjeta_credito_clean.csv`: dataset limpio generado por el ETL.
- `Proyecto_INA.pbix`: dashboard desarrollado en Power BI.

## Flujo de trabajo

`Dataset original` → `EDA y auditoría de calidad` → `ETL y dataset limpio` → `Dashboard Power BI`

De esta manera, las visualizaciones y conclusiones se construyen sobre una versión de los datos cuya calidad fue revisada y cuyas transformaciones quedaron documentadas.
