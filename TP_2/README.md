# TP 2 – Clasificación: predicción de lluvia en Australia

Trabajo práctico de clasificación de **Aprendizaje Automático 1** — Tecnicatura en
Inteligencia Artificial (FCEIA, UNR): predecir, a las 23:59 de cada día, si al día
siguiente va a llover (`RainTomorrow`).

**Integrantes:** Maragliano Franco, Cura Valentin, Cortinas Bautista

## Estado de la entrega

| Entrega | Ítems | Estado |
|---|---|---|
| 1.ª | 1. Análisis descriptivo · 2. Regresión logística · 3. Modelos base | Entregado |
| 2.ª | 4. Optimización de hiperparámetros · 5. Explicabilidad · 6. AutoML | Entregado |
| 3.ª | 7. Redes neuronales · 8. Comparación · 9. MLOps (Docker) · 10. Conclusión | Pendiente |

## Contenido

| Archivo | Descripción |
|---|---|
| `TP-clasificacion-AA1.ipynb` | Notebook de trabajo, ya ejecutado (los resultados se ven sin correrlo) |
| `weatherAUS_2026C2.csv` | Dataset provisto por la cátedra, sin modificar |
| `requisitos.txt` | Dependencias de Python |

## Cómo ejecutarlo

```bash
py -3.11 -m venv .venv
.venv\Scripts\python -m pip install -r requisitos.txt
```

Abrir `TP-clasificacion-AA1.ipynb`, elegir el kernel de `.venv` y ejecutar todas las
celdas (tarda unos 6 minutos en una PC de 16 núcleos). El notebook lee el CSV de esta misma
carpeta; si no lo encuentra (por ejemplo, al abrirlo en Google Colab desde GitHub), lo
descarga de este repositorio.

Se ejecutó con Python 3.11.9, PyCaret 3.3.2, scikit-learn 1.4.2, pandas 2.1.4,
LightGBM 4.3.0 y SHAP 0.49.1. Se usa Python 3.11 porque PyCaret 3.3 no funciona con
versiones más nuevas. Los resultados son reproducibles: la semilla está fijada en
`RANDOM_STATE = 42`, y dos ejecuciones completas desde un kernel limpio dieron las mismas
salidas.

## Resumen de la metodología

- **División temporal:** entrenamiento hasta 2014, validación 2015 y prueba desde 2016.
  Se divide por fecha porque el modelo siempre predice días futuros.
- **Datos:** se descarta `RainfallTomorrow` (es información del día siguiente y define el
  target) y solo se eliminan las 3.259 filas sin target. Los faltantes se imputan con la
  mediana del entrenamiento y un indicador de faltante, dentro de un `Pipeline` de
  scikit-learn.
- **Métrica principal:** F1 de la clase positiva (`RainTomorrow = Yes`), definida antes
  de ver los resultados.
- **Validación cruzada temporal:** 5 folds dentro del entrenamiento, cortados por fecha, los
  mismos para todas las comparaciones (representación de `Location`, preprocesamiento,
  hiperparámetros y PyCaret).
- **`Location`:** se reemplaza por la latitud y la longitud de cada estación (2 columnas en
  lugar de 49 one-hot), contrastadas con el listado de estaciones de la Oficina de
  Meteorología de Australia. La elección se comparó con one-hot, polinomios de las
  coordenadas y regiones por KMeans (sección 1.11).
- **Modelos:** regresión logística (optimizada con grid search) y LightGBM (optimizado con
  random search), con el umbral elegido en validación. PyCaret se usa como referencia, con
  los mismos folds. El modelo final se elige en validación, nunca en prueba.
- **Resultado en prueba (F1):** LightGBM optimizado con umbral 0,31 → 0,673; mejor
  regresión logística (umbral 0,26) → 0,618; LightGBM de PyCaret con umbral 0,28 → 0,656.
  El mejor modelo base, la persistencia ("mañana llueve si hoy llovió"), obtiene 0,447.
