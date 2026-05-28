# Consigna (b) — Modelo con embeddings

**Puntos:** 3  
**Notebook:** `Luna-Marcelo-DL-TP2-Co24.ipynb` (sección `## b)`)

## Objetivo de la consigna

Implementar el pipeline de transformaciones definido en **(a)**, entrenar una red neuronal que use **al menos una capa de embedding** para variables categóricas, y evaluar el desempeño en validación con métricas, curvas de entrenamiento y matrices de confusión.

## Dependencias previas

- Tabla de transformaciones por variable completada en **(a)**.
- `df_entrenamiento`, `df_validacion`, `columnas_predictoras` y `COLUMNA_OBJETIVO` disponibles (Fase 0).
- Decisiones explícitas: qué columnas van a **embedding**, cuáles a label/ordinal, cuáles a one-hot y cómo se escalan las numéricas.

## Criterio del TP (recordatorio)

- **≥ 1 capa `Embedding`** en el modelo (puede haber más de una).
- Dimensiones de embedding **justificadas** (no tienen que ser iguales entre variables).
- **Dropout** en capas ocultas.
- Optimizador **Adam** o variante.
- Función de costo: **Binary CrossEntropyLoss** o **Categorical CrossEntropyLoss** según cómo se codifique el target.
- Mismas métricas y gráficos que luego se repetirán en **(c)** para comparar.

## Reglas de trabajo (acordadas)

1. Usar solo `adult_train.csv` / `adult_val.csv` (repo o carga explícita en Colab).
2. Notebook ejecutable en Colab; código, gráficos, prints y explicaciones guardados en el mismo archivo.
3. Respuestas **debajo del enunciado (b)**; este archivo es guía de planificación.

---

## Lista de tareas / pasos

### Fase 0 — Preparación (reutilizar de consigna a)

- [ ] **0.1** Imports adicionales: `torch`, `torch.nn`, `DataLoader`, `sklearn.metrics`, etc.
- [ ] **0.2** Fijar semilla (`random`, `numpy`, `torch`) para reproducibilidad.
- [ ] **0.3** Detectar dispositivo (`cuda` / `cpu`) y documentar en una línea.

### Fase 1 — Preprocesado según decisiones de (a)

- [ ] **1.1** Codificar **target** `income` (p. ej. `<=50K` → 0, `>50K` → 1) y justificar la loss elegida.
- [ ] **1.2** Aplicar transformaciones a **numéricas** (escalado, log1p, etc.) usando estadísticas **solo de train**; aplicar el mismo transformador a val.
- [ ] **1.3** Variables categóricas **no embebidas**: label/ordinal o one-hot según tabla de (a); construir vocabularios/mapeos con train y manejar categorías nuevas en val (categoría `Desconocido` o política definida).
- [ ] **1.4** Variables categóricas **embebidas**: mapear cada categoría a un índice entero (`0 … n_categories-1`); guardar `cardinalities` por columna.
- [ ] **1.5** Unificar entradas del modelo: tensores numéricos + índices para embeddings + (si aplica) bloques one-hot/ordinal concatenados.
- [ ] **1.6** Crear `Dataset` / `DataLoader` para train y val (batch size definido y comentado).

### Fase 2 — Arquitectura del modelo

- [ ] **2.1** Definir clase `nn.Module` con:
  - una o más capas `nn.Embedding` (dims fundamentadas en markdown),
  - capas densas ocultas (número de capas, neuronas y activación documentados),
  - **Dropout** en ocultas,
  - capa de salida acorde a clasificación binaria (1 logit + sigmoid implícita en BCE, o 2 logits con CE).
- [ ] **2.2** Calcular y mostrar **cantidad de parámetros** totales (y por bloque: embeddings vs densas).
- [ ] **2.3** Calcular y mostrar **dimensión de entrada** efectiva al primer layer denso (para usar en consigna d).

### Fase 3 — Entrenamiento

- [ ] **3.1** Configurar optimizador **Adam** (lr y hiperparámetros elegidos; breve justificación).
- [ ] **3.2** Implementar loop de entrenamiento y validación por época.
- [ ] **3.3** Registrar por época, en train y val: **accuracy** y **F1 macro** (usar `sklearn` o implementación consistente).
- [ ] **3.4** Elegir criterio de parada (nº de épocas fijo o early stopping) y documentarlo.

### Fase 4 — Visualizaciones de entrenamiento

- [ ] **4.1** Gráfico **accuracy vs epoch** (curvas train y val en el mismo plot).
- [ ] **4.2** Gráfico **F1 macro vs epoch** (curvas train y val en el mismo plot).
- [ ] **4.3** Comentar brevemente si hay sobreajuste (gap train–val).

### Fase 5 — Evaluación en validación

- [ ] **5.1** Obtener predicciones finales en **val** (modelo en eval mode).
- [ ] **5.2** `classification_report` de sklearn (precision, recall, f1 por clase y promedios).
- [ ] **5.3** Matriz de confusión **absoluta** (val) con paleta legible y etiquetas de clase.
- [ ] **5.4** Matriz de confusión **normalizada por fila** (val); mismo estilo visual.
- [ ] **5.5** Guardar en variables los resultados clave (métricas, n_params, input_dim) para la tabla de **(d)**.

### Fase 6 — Cierre de la consigna en el notebook

- [ ] **6.1** Markdown: resumen de arquitectura, dimensiones de embedding y formulación de la loss.
- [ ] **6.2** Verificar que todas las salidas quedan guardadas (`Run all` en Colab).
- [ ] **6.3** Releer enunciado **(b)** y marcar checklist debajo de `## b)`.

---

## Checklist de cumplimiento (enunciado original)

| Requisito del enunciado | Tarea(s) relacionada(s) |
|-------------------------|-------------------------|
| Transformaciones de (a) en el flujo de entrenamiento | Fase 1 |
| Al menos una capa de embedding | 2.1 |
| Dimensión de embedding fundamentada | 2.1, 6.1 |
| Arquitectura libre (capas, neuronas, activación) | 2.1 |
| Dropout en ocultas | 2.1 |
| Adam o variante | 3.1 |
| BCE o CE según formulación | 1.1, 3.1 |
| Curvas accuracy y F1 macro (train y val) | 4.1–4.2 |
| Classification report (sklearn) | 5.2 |
| Matriz de confusión absoluta y normalizada por fila (val) | 5.3–5.4 |

---

## Entregables a conservar para consigna (c) y (d)

| Artefacto | Uso posterior |
|-----------|----------------|
| Hiperparámetros de capas ocultas (neuronas, activaciones, nº capas) | Replicar arquitectura en **(c)** |
| Lista de variables que iban a embedding | Pasarlas a one-hot en **(c)** |
| `n_params`, `input_dim`, métricas finales en val | Tabla comparativa en **(d)** |
| Curvas y matrices del modelo B | Contraste visual con modelo C |

---

## Orden sugerido de implementación en el notebook

1. Fase 0 → Fase 1 (preprocesado completo y DataLoaders)  
2. Fase 2 (modelo + conteo de parámetros)  
3. Fase 3 → Fase 4 (entrenar y graficar curvas)  
4. Fase 5 → Fase 6 (evaluación final y texto de cierre)

---

## Planificación del TP completo

| Consigna | Archivo |
|----------|---------|
| **(a)** EDA y transformaciones | [consigna-a.md](consigna-a.md) |
| **(b)** Modelo con embeddings *(este documento)* | [consigna-b.md](consigna-b.md) |
| **(c)** Modelo one-hot sin dropout | [consigna-c.md](consigna-c.md) |
| **(d)** Conclusiones comparativas | [consigna-d.md](consigna-d.md) |
