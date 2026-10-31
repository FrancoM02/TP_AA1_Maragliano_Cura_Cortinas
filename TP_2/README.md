# TP 2 – Clasificación: predicción de lluvia en Australia

Trabajo práctico de clasificación de **Aprendizaje Automático 1** — Tecnicatura en
Inteligencia Artificial (FCEIA, UNR): predecir, a las 23:59 de cada día, si al día
siguiente va a llover (`RainTomorrow`).

**Integrantes:** Maragliano Franco, Cura Valentin, Cortinas Bautista

## Estado de la entrega

| Entrega | Ítems | Estado |
|---|---|---|
| 1.ª | 1. Análisis descriptivo · 2. Regresión logística · 3. Modelos base | Entregado |
| 2.ª | 4. Optimización de hiperparámetros · 5. Explicabilidad · 6. AutoML | Pendiente |
| 3.ª | 7. Redes neuronales · 8. Comparación · 9. MLOps (Docker) · 10. Conclusión | Pendiente |

## Contenido

| Archivo | Descripción |
|---|---|
| `TP-clasificacion-AA1.ipynb` | Notebook de trabajo, ya ejecutado (los resultados se ven sin correrlo) |
| `weatherAUS_2026C2.csv` | Dataset provisto por la cátedra, sin modificar |
| `requisitos.txt` | Dependencias de Python |

## Cómo ejecutarlo

```bash
python -m venv .venv
.venv\Scripts\python -m pip install -r requisitos.txt
```

Abrir `TP-clasificacion-AA1.ipynb`, elegir el kernel de `.venv` y ejecutar todas las
celdas (tarda menos de un minuto). El notebook lee el CSV de esta misma carpeta; si no lo
encuentra (por ejemplo, al abrirlo en Google Colab desde GitHub), lo descarga de este
repositorio.

Se ejecutó con Python 3.13, pandas 3.1, scikit-learn 1.9, matplotlib 3.11 y seaborn 0.13.
Los resultados son reproducibles: la semilla está fijada en `RANDOM_STATE = 42`.

## Resumen de la metodología

- **División temporal:** entrenamiento hasta 2014, validación 2015 y prueba desde 2016.
  Se divide por fecha porque el modelo siempre predice días futuros.
- **Datos:** se descarta `RainfallTomorrow` (es información del día siguiente y define el
  target) y solo se eliminan las 3.259 filas sin target. Los faltantes se imputan con la
  mediana del entrenamiento y un indicador de faltante, dentro de un `Pipeline` de
  scikit-learn.
- **Métrica principal:** F1 de la clase positiva (`RainTomorrow = Yes`), definida antes
  de ver los resultados.
- **Resultado en prueba:** la regresión logística con el umbral 0,32 (elegido en
  validación) obtiene F1 = 0,637 y ROC-AUC = 0,861. El mejor modelo base, la persistencia
  ("mañana llueve si hoy llovió"), obtiene F1 = 0,447.
