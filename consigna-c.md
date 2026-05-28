# Consigna (c) — Modelo sin embeddings (one-hot)

**Puntos:** 3  
**Notebook:** `Luna-Marcelo-DL-TP2-Co24.ipynb` (sección `## c)`)

## Objetivo de la consigna

Entrenar un **segundo modelo** comparable al de **(b)**, reemplazando las variables que iban a **embedding** por **one-hot encoding**, **sin dropout** ni otra regularización interna, y presentar las **mismas métricas y visualizaciones** que en el modelo anterior.

## Dependencias previas

- Modelo **(b)** entrenado y documentado (arquitectura, hiperparámetros, resultados en val).
- Tabla de **(a)**: identificar qué columnas eran **embedding en (b)** → deben ser **one-hot en (c)**.
- Resto de transformaciones (numéricas, otras categóricas) **igual que en (b)**, salvo el cambio embedding → one-hot.

## Criterio del TP (recordatorio)

- **One-hot** en todas las variables que en **(b)** usaban embedding.
- **Sin dropout** (ni otra regularización en capas internas).
- **Misma arquitectura** que **(b)**: mismo número de capas ocultas, mismas neuronas por capa, mismas funciones de activación.
- Mismas salidas que **(b)**: curvas accuracy / F1 macro, classification report, matrices de confusión (absoluta y normalizada por fila en val).

## Reglas de trabajo (acordadas)

1. Usar solo los datasets provistos / cargados por el usuario.
2. Todo en el notebook ejecutable en Colab, con salidas guardadas.
3. Respuestas **debajo del enunciado (c)**; este archivo es guía de planificación.

---

## Lista de tareas / pasos

### Fase 0 — Alineación con el modelo (b)

- [ ] **0.1** Listar explícitamente las variables que pasan de **embedding → one-hot** (copiar de tabla de (a)/(b)).
- [ ] **0.2** Anotar arquitectura de **(b)**: capas ocultas, `in_features` / `out_features`, activaciones, sin dropout.
- [ ] **0.3** Reutilizar semilla y configuración de entrenamiento de **(b)** cuando sea posible (mismas épocas, lr, batch size) para comparación justa.

### Fase 1 — Preprocesado one-hot

- [ ] **1.1** Ajustar one-hot solo para las columnas que eran embedding en **(b)** (`pd.get_dummies`, `OneHotEncoder`, etc.).
- [ ] **1.2** Ajustar vocabulario/categorías con **train**; aplicar la misma política de categoría `Desconocido` en val que en **(b)**.
- [ ] **1.3** Recalcular **dimensión total de entrada** al MLP (numéricas + one-hot nuevas + resto de codificaciones).
- [ ] **1.4** Verificar que train y val tienen **mismas columnas** tras one-hot (alinear columnas faltantes en val con 0 si aplica).
- [ ] **1.5** Reconstruir `Dataset` / `DataLoader` para train y val con el nuevo tamaño de entrada.

### Fase 2 — Arquitectura (espejo de b, sin dropout)

- [ ] **2.1** Definir modelo **sin** `nn.Embedding` y **sin** `nn.Dropout`.
- [ ] **2.2** Igualar capas densas a **(b)**: mismas unidades y activaciones; adaptar solo la **primera capa lineal** al nuevo `input_dim`.
- [ ] **2.3** Mostrar **cantidad de parámetros** y comparar con modelo **(b)** (comentario preliminar).
- [ ] **2.4** Documentar `input_dim` del modelo C para tabla de **(d)**.

### Fase 3 — Entrenamiento

- [ ] **3.1** Mismo optimizador (**Adam** o variante) y misma loss que en **(b)**.
- [ ] **3.2** Loop de entrenamiento / validación por época (misma cantidad de épocas salvo justificación).
- [ ] **3.3** Registrar accuracy y F1 macro en train y val por época.

### Fase 4 — Visualizaciones de entrenamiento

- [ ] **4.1** Curva **accuracy vs epoch** (train y val).
- [ ] **4.2** Curva **F1 macro vs epoch** (train y val).
- [ ] **4.3** Comentar diferencias de convergencia respecto a **(b)** si son visibles.

### Fase 5 — Evaluación en validación

- [ ] **5.1** Predicciones en **val** con modelo en eval mode.
- [ ] **5.2** `classification_report` (sklearn).
- [ ] **5.3** Matriz de confusión **absoluta** (val).
- [ ] **5.4** Matriz de confusión **normalizada por fila** (val).
- [ ] **5.5** Guardar métricas finales, `n_params` e `input_dim` para **(d)**.

### Fase 6 — Cierre de la consigna en el notebook

- [ ] **6.1** Markdown: explicar el cambio embedding → one-hot y por qué cambia el tamaño de entrada / parámetros.
- [ ] **6.2** Confirmar explícitamente que **no** hay dropout ni regularización interna.
- [ ] **6.3** `Run all` y revisión del checklist bajo `## c)`.

---

## Checklist de cumplimiento (enunciado original)

| Requisito del enunciado | Tarea(s) relacionada(s) |
|-------------------------|-------------------------|
| One-hot en vars que eran embedding en (b) | Fase 1 |
| Sin dropout / sin regularización interna | 2.1, 6.2 |
| Misma arquitectura que (b) salvo entrada y embeddings | 2.2 |
| Mismas métricas y visualizaciones que (b) | Fases 4–5 |

---

## Puntos de control (comparación justa con b)

| Aspecto | Modelo (b) | Modelo (c) |
|---------|------------|------------|
| Vars en embedding | Sí (≥1) | No — one-hot |
| Dropout | Sí | No |
| Capas ocultas / neuronas / activación | Referencia | Debe coincidir |
| Optimizador y loss | Referencia | Igual |
| Métricas y gráficos | Referencia | Mismos |

---

## Entregables a conservar para consigna (d)

- Métricas finales en val (accuracy, F1 macro, precision/recall por clase si se desea).
- `n_params` modelo B y modelo C.
- `input_dim` modelo B y modelo C.
- Observaciones sobre sobreajuste / velocidad de entrenamiento.

---

## Orden sugerido de implementación en el notebook

1. Fase 0 (checklist de equivalencia con b)  
2. Fase 1 (one-hot y nuevos DataLoaders)  
3. Fase 2 → Fase 3 → Fase 4  
4. Fase 5 → Fase 6

---

## Planificación del TP completo

| Consigna | Archivo |
|----------|---------|
| **(a)** EDA y transformaciones | [consigna-a.md](consigna-a.md) |
| **(b)** Modelo con embeddings | [consigna-b.md](consigna-b.md) |
| **(c)** Modelo one-hot sin dropout *(este documento)* | [consigna-c.md](consigna-c.md) |
| **(d)** Conclusiones comparativas | [consigna-d.md](consigna-d.md) |
