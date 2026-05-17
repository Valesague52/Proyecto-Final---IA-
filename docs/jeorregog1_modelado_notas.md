# jeorregog1 - Modelado, Arquitectura y Metodología

Notas de trabajo para el bloque de **jeorregog1**.

## Confirmación del enfoque del proyecto

Sí, el proyecto quedó alineado con la **opción 1** que habíamos propuesto:

- **Tema:** predicción de deserción estudiantil.
- **Dataset:** Higher Education Predictors of Student Retention / Student Dropout (Kaggle).
- **Tipo de problema:** clasificación binaria de riesgo de deserción.
- **Modelo principal acordado por el equipo:** **LightGBM**.
- **Salida de tu bloque:** un modelo LightGBM tuneado y listo para que el Integrante 3 lo use en la evaluación final y la comunicación de resultados.

## Alcance de jeorregog1

Tu bloque queda así:

- Construir el **baseline**.
- Entrenar el **modelo principal LightGBM**.
- Hacer **tuning** sin romper la separación train/val/test.
- Generar artefactos de **validación y tuning**.
- Redactar las **secciones 3 y 4 del informe**.

La imagen de reparto y la división textual del proyecto se pueden reconciliar sin solaparte con el Integrante 3:

- El texto formal de la división menciona baseline + modelo principal + tuning.
- La imagen resume el bloque de forma más amplia, pero para evitar mezclar responsabilidades conviene cerrar tu parte en el **módulo predictivo**.

La forma práctica de organizar tu parte es:

1. baseline
2. LightGBM base
3. LightGBM tuneado
4. handoff del mejor modelo al Integrante 3

## Sección 3 del informe - Arquitectura del sistema

### Idea general

El sistema se divide en tres capas:

1. **Entrada y preparación de datos**
2. **Motor predictivo**
3. **Salida de probabilidades y predicciones**

### Texto base sugerido

La arquitectura del sistema se diseñó como un flujo modular orientado a la predicción del riesgo de deserción académica. En una primera etapa, el sistema recibe variables académicas, demográficas, financieras e institucionales provenientes del dataset de estudiantes. Estas variables son procesadas mediante el pipeline de limpieza y transformación construido en la fase anterior del proyecto, garantizando consistencia en escalado, partición de datos y trazabilidad experimental.

En la segunda etapa se implementó el módulo de modelado, compuesto por un baseline de referencia y un modelo principal basado en LightGBM. La elección de LightGBM responde a tres razones: su buen desempeño en datos tabulares, su robustez frente a relaciones no lineales y su capacidad para manejar de forma eficiente interacciones complejas entre variables.

Finalmente, el módulo predictivo entrega probabilidades de riesgo y predicciones binarias que pueden ser reutilizadas en etapas posteriores del proyecto. De esta manera, el Integrante 3 puede tomar el mejor modelo seleccionado y usarlo como base para la evaluación final, la comparación formal de modelos y la comunicación de resultados.

### Componentes que debes mencionar

- **Datos de entrada:** variables procesadas por el Integrante 1.
- **Baseline:** referencia simple para comparar desempeño.
- **Modelo principal:** LightGBM con manejo de desbalance.
- **Interfaz de salida para el Integrante 3:** probabilidad de dropout, clase predicha y mejor configuración encontrada.

### Figura sugerida para esta sección

- Diagrama simple:
  - `Datos procesados -> Baseline / LightGBM -> Probabilidad de deserción -> Handoff a evaluación final`

## Sección 4 del informe - Metodología

### Idea general

Esta sección debe explicar:

- cómo se entrenó
- cómo se validó
- cómo se eligió el modelo final
- con qué métricas se comparó

### Texto base sugerido

El proceso de modelado se llevó a cabo sobre los conjuntos de entrenamiento y validación generados en la fase de preprocesamiento. Para evitar data leakage, la selección de hiperparámetros se realizó utilizando únicamente la información de entrenamiento y validación, reservando el conjunto de prueba exclusivamente para la evaluación final que desarrollará el Integrante 3.

Como punto de referencia se entrenó un baseline simple, con el objetivo de establecer un nivel mínimo de desempeño. Posteriormente se implementó un modelo LightGBM con `class_weight='balanced'`, decisión justificada por el desbalance detectado en la variable objetivo durante el análisis exploratorio. Después se realizó un proceso de ajuste de hiperparámetros para optimizar el desempeño del modelo sobre el conjunto de validación.

La selección del modelo final se basó principalmente en la métrica F1, complementada con AUC-ROC, recall, precision y accuracy. Esta elección responde a que el problema de deserción académica presenta desbalance de clases y el interés práctico está en detectar correctamente a los estudiantes en riesgo sin depender solo de accuracy.

### Decisiones metodológicas que debes defender

- Problema tratado como **clasificación binaria**.
- División respetada:
  - `train`: ajuste inicial
  - `val`: selección/tuning
  - `test`: reservado para evaluación final
- **Manejo del desbalance** con `class_weight='balanced'`.
- Baseline para medir ganancia real.
- Tuning de LightGBM con búsqueda aleatoria.

### Métricas recomendadas

- **F1** como métrica principal.
- **AUC-ROC** como apoyo.
- **Recall** porque perder estudiantes en riesgo puede ser costoso.
- **Precision** para medir calidad de las alertas.
- **Accuracy** solo como referencia secundaria.

## Qué debes completar después de correr el notebook 03

- Nombre exacto del baseline usado en el informe.
- Mejores hiperparámetros encontrados.
- Métricas de validación del modelo tuneado.
- Comparación numérica en validación contra baseline.
- Ruta del modelo guardado para entregarlo al Integrante 3.
- Resumen corto de handoff en JSON con rutas y mejor candidato.

## Figuras sugeridas para tu bloque

- `fig8_validation_model_comparison.png`: comparación de métricas entre candidatos en validación.

## Secuencia honesta de commits para tu bloque

Sin falsear fechas ni “simular” trabajo, una secuencia limpia puede ser:

1. `chore(modeling): update requirements for the LightGBM notebook`
2. `feat(modeling): add notebook 03 with baselines and LightGBM workflow`
3. `docs(modeling): add architecture and methodology notes for jeorregog1`
4. `feat(modeling): add tuned LightGBM artifacts for handoff`

## Artefactos de handoff sugeridos

Además del modelo `.pkl`, es útil dejar:

- `metrics_validation_candidates.csv`
- `lightgbm_best_params.json`
- `lightgbm_tuning_results.csv`
- `modeling_handoff_summary.json`

Ese último archivo le facilita al Integrante 3 identificar rápido:

- cuál fue el mejor candidato por validación
- cuál fue la métrica de selección
- dónde están los parámetros finales
- dónde está el modelo serializado

## Resultados de la corrida de 03_modeling

La corrida final del notebook ya se ejecuto correctamente en el entorno local con Python 3.12 y dejo estos resultados principales:

- Mejor candidato por `f1` en validacion: `lightgbm_tuned`
- `f1`: `0.8057`
- `accuracy`: `0.8765`
- `precision`: `0.8173`
- `recall`: `0.7944`
- `roc_auc`: `0.9235`

Comparacion directa contra el baseline mas competitivo:

- `logreg_balanced`: `f1 = 0.8054`
- `lightgbm_tuned`: `f1 = 0.8057`

La mejora del modelo tuneado sobre la regresion logistica balanceada es pequena, pero suficiente para justificar la seleccion final si el criterio principal del proyecto es `f1`.

## Handoff final

Artefactos listos para el Integrante 3:

- `models/metrics_validation_candidates.csv`
- `models/lightgbm_best_params.json`
- `models/lightgbm_tuning_results.csv`
- `models/modeling_handoff_summary.json`
- `data/processed/fig8_validation_model_comparison.png`

El modelo serializado `models/checkpoints/lightgbm_dropout_model.pkl` se genero correctamente de forma local, pero no queda versionado porque esta ignorado en `.gitignore`.
