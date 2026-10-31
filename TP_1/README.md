# TP_AA1_Maragliano_Cura_Cortinas

Trabajo práctico de regresión de **Aprendizaje Automático 1** — Tecnicatura en
Inteligencia Artificial (FCEIA, UNR): predicción del precio de viviendas en Boston.

**Integrantes:** Maragliano Franco, Cura Valentin, Cortinas Bautista

## Contenido

| Archivo | Descripción |
|---|---|
| `TP-regresion-AA1.ipynb` | Notebook de trabajo, ya ejecutado (se pueden ver los resultados sin correrlo) |
| `house-prices-tp.csv` | Dataset original provisto por la cátedra, sin modificar |
| `house-prices-tp-clean.csv` | Dataset luego de la limpieza: 506 filas, sin las 50 filas incompletas (lo genera el notebook) |
| `requisitos.txt` | Dependencias de Python |

## Cómo ejecutarlo

```bash
python -m venv .venv
.venv\Scripts\python -m pip install -r requisitos.txt
```

Abrir `TP-regresion-AA1.ipynb`, elegir el kernel de `.venv` y ejecutar todas las
celdas. También corre en Google Colab subiendo el CSV a `/content/sample_data/`.
