# Consigna (a) — Análisis exploratorio y decisiones de transformación

**Puntos:** 2  
**Notebook:** `Luna-Marcelo-DL-TP2-Co24.ipynb` (sección `## a)`)

## Objetivo de la consigna

Realizar un EDA del dataset Adult Census Income (train y val), analizar las variables categóricas (valores únicos y cardinalidad) y **definir y justificar** la transformación que se aplicará a **cada** variable antes de entrenar los modelos de las consignas (b) y (c).

## Criterio del TP (recordatorio)

- **Alta cardinalidad:** variable categórica con **≥ 10** valores únicos.
- No hace falta usar la misma transformación en todas las categóricas; se esperan **distintos enfoques** según cardinalidad, contexto y orden lógico.
- Opciones de transformación: **label/ordinal encoding**, **one-hot encoding**, **embedding** (en el modelo, consigna b).

## Reglas de trabajo (acordadas)

1. Usar solo `adult_train.csv` / `adult_val.csv` (repo o carga explícita en Colab); no asumir equivalencia con datasets de internet.
2. El desarrollo final vive en el notebook ejecutable en Colab, con gráficos, prints y texto guardados.
3. Las respuestas al corrector van **debajo del enunciado (a)** en el notebook; este archivo es guía de planificación.

---

## Lista de tareas / pasos

### Fase 0 — Setup y carga de datos ✅ *completada*

*Implementado en el notebook: celdas **0.1** (setup), **0.2** (carga) y **0.3–0.5** (verificación inicial).*

- [x] **0.1** Celda de setup Colab: imports (`pandas`, `numpy`, `matplotlib`/`seaborn`, etc.) y configuración de visualización.
- [x] **0.2** Cargar `adult_train.csv` y `adult_val.csv` desde el directorio de trabajo; si faltan, subida ordenada en Colab (train → val).
- [x] **0.3** Verificar forma de los datos: `shape`, tipos (`dtypes`), duplicados, valores faltantes o marcadores especiales (p. ej. `?`, strings vacíos).
- [x] **0.4** Confirmar que **train** y **val** comparten las mismas columnas y que `income` es la variable objetivo.
- [x] **0.5** Separar features vs target en código (`columnas_predictoras`).

### Fase 1 — Panorama general del dataset ✅ *completada*

*Notebook: 1.1–1.2, 1.3 (target), 1.4 (extremos), 1.5 (cardinalidad), 1.6 / 1.6b / 1.7 (distribuciones gráficas).*

- [x] **1.1** Resumen numérico: `describe()` para variables continuas/discretas (`age`, `capital-gain`, `capital-loss`, `hours-per-week`).
- [x] **1.2** Conteo de filas por split (train / val) y proporción respecto del total.
- [x] **1.3** Análisis del **target** `income`: conteos, proporciones, gráfico de barras; comentar si hay desbalance de clases.
- [x] **1.4** Detectar posibles outliers o valores extremos en numéricas (boxplots, histogramas o percentiles).
- [x] **1.5** Tabla de **valores únicos** por variable categórica e identificación de **alta cardinalidad** (≥ 10 valores únicos).

### Fase 2 — EDA por tipo de variable (gráficas coherentes) ✅ *completada*

#### Numéricas ✅ *completadas (2.1–2.3)*

- [x] **2.1** Histogramas y/o KDE de `age`, `capital-gain`, `capital-loss`, `hours-per-week` (train). → Notebook **1.7** (val opcional en **2.9**).
- [x] **2.2** Boxplots de numéricas vs `income` (relación con la clase). → Notebook **1.4**.
- [x] **2.3** Matriz de correlación (Pearson/Spearman) entre numéricas; comentar correlaciones relevantes (sin asumir causalidad). → Notebook **2.3** (Fase 2).

#### Categóricas — inventario y cardinalidad

Para cada columna categórica del enunciado:

`workclass`, `education`, `marital-status`, `occupation`, `relationship`, `race`, `sex`, `skill-profile`, `native-country`

- [x] **2.4** Listar **valores únicos** (ordenados o por frecuencia), conteo y porcentaje por categoría. → Notebook **2.4** (Fase 2).
- [x] **2.5** Calcular **cardinalidad** (# valores únicos) y clasificar: **baja** (< 10) / **alta (≥ 10)** según criterio del TP. → Notebook **1.5** (el TP no define categoría “media”).
- [x] **2.6** Gráficos de frecuencia: barras horizontales o top-N + “otros” si hay muchas categorías raras. → Notebooks **1.6** y **1.6b**.
- [x] **2.7** Cruce con target: barras apiladas, proporción de `>50K` por categoría o heatmap simple; identificar categorías más asociadas a alta renta. → Notebook **2.7** (Fase 2).
- [x] **2.8** Revisar categorías **poco frecuentes** en train: ¿aparecen en val? ¿conviene agrupar, mantener o tratar en missing? → Notebook **2.8** (Fase 2).

#### Calidad y consistencia ✅ *completada (2.9–2.10)*

- [x] **2.9** Comparar distribuciones train vs val (target y 2–3 features clave) para detectar **data shift** evidente. → Notebook **2.9** (Fase 2).
- [x] **2.10** Documentar hallazgos que impacten el preprocesado (nulos, typos, categorías con una sola observación, etc.). → Notebook **2.10** (markdown, cierre Fase 2).

### Fase 3 — Criterios para elegir transformación (por variable) ✅ *completada*

Para **cada** feature (numéricas y categóricas), evaluar explícitamente:

- [x] **3.1** **Cardinalidad** (especialmente ≥ 10 → candidata a embedding u otra estrategia que no explote dimensión). → Notebook **3.1** (markdown; datos de celda **1.5**).
- [x] **3.2** **Contexto semántico** (¿nominal sin orden? ¿ordinal con orden natural?, p. ej. `education`). → Notebook **3.2** (markdown; orden en celda **1.6**).
- [x] **3.3** **Relación con el target** (señal predictiva fuerte o débil). → Notebook **3.3** (markdown; celda **2.7** y **1.4**).
- [x] **3.4** **Estabilidad train/val** (categorías vistas solo en un split). → Notebook **3.4** (markdown; celdas **2.8** y **2.9**).
- [x] **3.5** Elegir una opción entre:
  - Sin transformación / **escalado** (numéricas),
  - **Label / ordinal encoding**,
  - **One-hot encoding**,
  - **Embedding** en modelo (consigna b; anotar dimensión tentativa y fundamento preliminar).
  → Notebook **3.5** (markdown; tabla por variable y dims. embedding).

> Nota: en (a) no es obligatorio implementar el pipeline completo; sí es obligatorio **decidir y justificar**. Opcional: celdas de prueba mínimas que validen shapes tras una codificación de ejemplo.

### Fase 4 — Tabla de decisiones finales (entregable escrito)

- [ ] **4.1** Redactar una **tabla o lista** con una fila por variable:

  | Variable | Tipo | Cardinalidad | Transformación elegida | Justificación breve | Uso en modelo (b) / (c) |
  |----------|------|--------------|------------------------|---------------------|------------------------|

- [ ] **4.2** Asegurar **variedad**: al menos dos tipos distintos de codificación entre categóricas (p. ej. ordinal + one-hot + embedding), alineado con el enunciado.
- [ ] **4.3** Marcar explícitamente qué variables irán a **embedding** en (b) y serán **one-hot** en (c).
- [ ] **4.4** Para numéricas: decidir escalado (StandardScaler, MinMax, log1p en gains, etc.) y justificar según distribución observada.

### Fase 5 — Cierre de la consigna en el notebook

- [ ] **5.1** Párrafo de síntesis del EDA: 3–5 hallazgos principales (balance, variables más informativas, problemas de calidad).
- [ ] **5.2** Verificar que todas las gráficas y prints quedan **guardados** en el notebook (ejecutar `Run all` en Colab antes de entregar).
- [ ] **5.3** Releer el enunciado (a) y comprobar que cada ítem del checklist está respondido **debajo** de `## a)`.

---

## Checklist de cumplimiento (enunciado original)

| Requisito del enunciado | Tarea(s) relacionada(s) |
|-------------------------|-------------------------|
| EDA con gráficas adecuadas y coherentes | Fase 2 |
| Valores únicos y cardinalidad de categóricas | 2.4–2.5 |
| Justificación detallada de transformaciones | Fase 3 |
| Distintos tipos de transformación según la variable | 3.5, 4.2 |
| Decisión final explícita por variable + justificación | 4.1–4.3, 5.1 |

---

## Variables del dataset (referencia)

| Variable | Descripción breve |
|----------|-------------------|
| `age` | Edad en años |
| `workclass` | Tipo de empleador / relación laboral |
| `education` | Nivel educativo |
| `marital-status` | Estado civil |
| `occupation` | Área u ocupación laboral |
| `relationship` | Relación con el jefe de hogar |
| `race` | Autoidentificación racial |
| `sex` | Sexo biológico |
| `capital-gain` | Ganancias de capital |
| `capital-loss` | Pérdidas de capital |
| `hours-per-week` | Horas trabajadas por semana |
| `skill-profile` | Habilidad / perfil técnico |
| `native-country` | País de nacimiento |
| `income` | Target: `<=50K` / `>50K` |

---

## Orden sugerido de implementación en el notebook

1. Fase 0 → Fase 1  
2. Fase 2 (numéricas, luego categóricas una por una o en bloque)  
3. Fase 3 + Fase 4 (texto/tabla de decisiones)  
4. Fase 5 (síntesis y revisión final)

---

## Planificación de las demás consignas

Las decisiones de transformación de **(a)** alimentan directamente los modelos siguientes. Guías de pasos:

| Consigna | Archivo | Depende de (a) |
|----------|---------|----------------|
| **(b)** Modelo con embeddings | [consigna-b.md](consigna-b.md) | Tabla de transformaciones; variables a embeber |
| **(c)** Modelo sin embeddings (one-hot) | [consigna-c.md](consigna-c.md) | Qué variables eran embedding en (b) |
| **(d)** Conclusiones comparativas | [consigna-d.md](consigna-d.md) | Resultados de (b) y (c) |
