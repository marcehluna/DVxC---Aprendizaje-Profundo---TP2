# Consigna (d) — Conclusiones finales

**Puntos:** 2  
**Notebook:** `Luna-Marcelo-DL-TP2-Co24.ipynb` (sección `## d)`)

## Objetivo de la consigna

Comparar de forma integral los modelos de **(b)** (con embeddings + dropout) y **(c)** (one-hot, sin dropout), elaborar una **tabla comparativa** (métricas, parámetros, tamaño de entrada) y redactar **conclusiones fundamentadas** sobre cuál enfoque rinde mejor y por qué, en el contexto de este problema de clasificación de ingresos.

## Dependencias previas

- Entrenamiento y evaluación completos de **(b)** y **(c)**.
- Valores guardados: métricas en val, `n_params`, `input_dim` (o equivalente) de ambos modelos.
- Decisiones de preprocesado de **(a)** (cardinalidad, variables embebidas, balance de clases).

## Criterio del TP (recordatorio)

- La tabla no debe limitarse a **precisión**: incluir **cantidad de parámetros** y **entradas** (dimensión del vector/tensores de entrada al MLP).
- El texto debe **justificar** el desempeño relativo usando características del problema (cardinalidad, dimensionalidad, regularización, desbalance, etc.).

## Reglas de trabajo (acordadas)

1. Conclusiones basadas en **tus** resultados y datasets, no en benchmarks genéricos de internet.
2. Tabla, gráficos comparativos (opcional) y texto en el **mismo notebook**, debajo de `## d)`.
3. Este archivo es guía de planificación.

---

## Lista de tareas / pasos

### Fase 0 — Recopilación de resultados

- [ ] **0.1** Extraer de **(b)** en validación: accuracy, F1 macro (y opcional: precision/recall por clase o weighted).
- [ ] **0.2** Extraer de **(c)** las mismas métricas en val.
- [ ] **0.3** Registrar **número total de parámetros** entrenables de cada modelo.
- [ ] **0.4** Registrar **dimensión de entrada** (o desglose: numéricas + one-hot + dims de embedding concatenadas) de cada modelo.
- [ ] **0.5** Anotar hiperparámetros compartidos (épocas, lr, batch size) para mencionar comparabilidad.

### Fase 1 — Tabla comparativa

- [ ] **1.1** Armar tabla con al menos estas filas/columnas:

  | | Modelo (b) embeddings + dropout | Modelo (c) one-hot sin dropout |
  |--|--------------------------------|------------------------------|
  | Accuracy (val) | | |
  | F1 macro (val) | | |
  | Parámetros totales | | |
  | Dimensión de entrada | | |
  | Variables en embedding / one-hot | | |

- [ ] **1.2** Incluir en markdown o celda el **mejor modelo** según F1 macro (o métrica principal elegida) y la diferencia numérica.
- [ ] **1.3** (Opcional) Gráfico de barras lado a lado para 2–3 métricas clave.

### Fase 2 — Análisis del tamaño del modelo y la entrada

- [ ] **2.1** Explicar por qué **(c)** suele tener más entradas si las vars de alta cardinalidad pasan a one-hot (relacionar con tabla de **(a)**).
- [ ] **2.2** Comparar **parámetros**: embeddings densos vs pesos de la primera capa lineal más ancha en **(c)**.
- [ ] **2.3** Comentar costo computacional / memoria si los datos lo justifican (breve).

### Fase 3 — Análisis del desempeño predictivo

- [ ] **3.1** Comparar curvas de entrenamiento de **(b)** y **(c)** (sobreajuste, estabilidad, velocidad).
- [ ] **3.2** Contrastar **classification reports** (¿una clase peor en ambos? ¿desbalance?).
- [ ] **3.3** Interpretar **matrices de confusión** (qué clase se confunde más; si es similar entre modelos).
- [ ] **3.4** Relacionar resultados con **dropout** presente solo en **(b)** y con el tipo de codificación categórica.

### Fase 4 — Conclusiones escritas (entregable principal)

- [ ] **4.1** Párrafo 1: resumen ejecutivo (qué modelo ganó y por cuánto en la métrica principal).
- [ ] **4.2** Párrafo 2: rol de **embeddings** frente a **one-hot** en variables de alta cardinalidad de este dataset.
- [ ] **4.3** Párrafo 3: efecto de **dropout** vs ausencia de regularización en **(c)** (según curvas train/val).
- [ ] **4.4** Párrafo 4: limitaciones del experimento (una sola arquitectura, sin búsqueda de hiperparámetros, etc.) y posible trabajo futuro.
- [ ] **4.5** Cierre alineado al objetivo del TP: clasificación de ingresos con datos censales.

### Fase 5 — Cierre de la consigna en el notebook

- [ ] **5.1** Verificar que la tabla y el texto están **debajo** de `## d)`.
- [ ] **5.2** `Run all` en Colab; salidas guardadas.
- [ ] **5.3** Releer enunciado **(d)** y marcar checklist.

---

## Checklist de cumplimiento (enunciado original)

| Requisito del enunciado | Tarea(s) relacionada(s) |
|-------------------------|-------------------------|
| Tabla comparativa con métricas, parámetros y entradas | Fase 1 |
| No solo precisión | 1.1 (incluir F1, params, input_dim) |
| Observaciones y apreciaciones detalladas | Fases 2–4 |
| Conclusiones fundamentadas (por qué uno es mejor/peor) | 4.1–4.5 |

---

## Preguntas guía para el texto (opcional)

- ¿El modelo con embedding generaliza mejor en categorías raras o de alta cardinalidad (`skill-profile`, `native-country`, etc.)?
- ¿El modelo one-hot memoriza más el train al no tener dropout?
- ¿El desbalance de `income` afecta por igual a ambos modelos?
- ¿La diferencia de desempeño justifica la complejidad extra de embeddings en este dataset?

---

## Orden sugerido de implementación en el notebook

1. Fase 0 (reunir números de b y c)  
2. Fase 1 (tabla + opcional gráfico)  
3. Fases 2–3 (análisis)  
4. Fase 4–5 (redacción final y revisión)

---

## Planificación del TP completo

| Consigna | Archivo |
|----------|---------|
| **(a)** EDA y transformaciones | [consigna-a.md](consigna-a.md) |
| **(b)** Modelo con embeddings | [consigna-b.md](consigna-b.md) |
| **(c)** Modelo one-hot sin dropout | [consigna-c.md](consigna-c.md) |
| **(d)** Conclusiones comparativas *(este documento)* | [consigna-d.md](consigna-d.md) |
