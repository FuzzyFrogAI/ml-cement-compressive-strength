# ml-cement-compressive-strength

> Pipeline completo de regresión para predecir la resistencia a la compresión del cemento según su composición química. EDA, reducción de dimensiones con PCA y VarianceThreshold, benchmarking de SVR, KNN y Random Forest, sintonización con GridSearchCV y despliegue con FastAPI.

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)

🇬🇧 [English](#english) | 🇪🇸 [Español](#español)

---

## English

### Overview

End-to-end regression pipeline to predict cement compressive strength from its chemical composition, without waiting 28 days for physical measurement.

- **Problem**: Given a cement sample's chemical composition (clinker, gypsum, limestone, pozzolan, etc.), predict its compressive strength at 28 days of curing.
- **Approach**: Feature selection with PCA and VarianceThreshold, model benchmarking, hyperparameter tuning, and REST API deployment.
- **Data**: 31 chemical composition attributes of Type ICo cement, with 4 target columns (RC at 1, 3, 7, and 28 days in PSI).

### What's inside

A multi-notebook pipeline covering the full ML workflow:

1. **Data ingestion** from Excel (chemical composition of Type ICo cement)
2. **EDA and correlation analysis** — Pearson matrix, scatter plots
3. **Dimensionality reduction** with PCA (10 components → 95%+ variance) and VarianceThreshold
4. **Model benchmarking** — SVR, KNN, and Random Forest with 5-fold cross-validation
5. **Hyperparameter tuning** with GridSearchCV on the best model (Random Forest)
6. **Evaluation** — MAE, RMSE, MAPE on a held-out test set
7. **Feature importance** — direct interpretability from Random Forest
8. **Deployment** as a REST API with FastAPI and pickle serialization

### Why this project

Measuring compressive strength physically requires casting test specimens and waiting 28 days. A machine learning model allows estimation from day 1 based on composition, accelerating quality control and reducing waste.

The dataset contains **31 highly correlated chemical attributes**, making it an ideal case for exploring feature selection and reduction techniques. The framework is replicable with the public [UCI Concrete Compressive Strength dataset](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength).

**Relevant for theses in civil or materials engineering**: it combines domain knowledge (materials chemistry) with applied ML, and the contribution is clear — model interpretability so the plant engineer understands which components most affect strength.

### Key results

| Model | MAE (PSI) | RMSE (PSI) | MAPE (%) |
|---|---|---|---|
| SVR | ~420 | ~560 | ~14% |
| KNN | ~380 | ~510 | ~12% |
| Random Forest (base) | ~280 | ~370 | ~9% |
| **Random Forest (GridSearchCV)** | **~235** | **~315** | **~7.5%** |

Tuning reduces RMSE by ~15% vs. the untuned baseline. **Clinker Conforme (%)** is the most important feature — consistent with cement chemistry.

### Quick start

```bash
git clone https://github.com/FuzzyFrogAI/ml-cement-compressive-strength.git
cd ml-cement-compressive-strength
pip install -r requirements.txt
jupyter notebook notebooks/01_eda.ipynb
```

To launch the prediction API:

```bash
uvicorn api.main:app --reload
# POST to http://localhost:8000/predecir with cement composition JSON
```

### Project structure

```
ml-cement-compressive-strength/
├── data/
│   └── composito_cemento_tipo_ico.xlsx
├── models/
│   ├── rf_cemento.pkl
│   └── scaler_cemento.pkl
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_feature_selection.ipynb
│   ├── 03_benchmarking.ipynb
│   └── 04_deploy.ipynb
├── src/
│   ├── preprocessing.py
│   └── model.py
├── api/
│   └── main.py
└── requirements.txt
```

### Tech stack

- `scikit-learn` — SVR, KNN, Random Forest, PCA, GridSearchCV, StandardScaler, VarianceThreshold
- `pandas` / `numpy` — data manipulation and numerical computation
- `matplotlib` — prediction plots and learning curves
- `fastapi` + `pydantic` — REST API deployment
- `pickle` — model serialization
- `openpyxl` — Excel ingestion

### Related resources

- **Full case study**: [fuzzyfrog.ai/es/ai-lab/proyectos/industria/prediccion-resistencia-cemento](https://www.fuzzyfrog.ai/es/ai-lab/proyectos/industria/prediccion-resistencia-cemento/)
- **Medium story**: [Your Cement Plant Is Waiting 28 Days for an Answer a Model Can Give in Seconds](https://medium.com/@alan.lopez.algorithmica/your-cement-plant-is-waiting-28-days-for-an-answer-a-model-can-give-in-seconds-1c8095b5f9bf)
- **Author**: [FuzzyFrog AI](https://fuzzyfrog.ai)

### License

MIT — see [LICENSE](LICENSE) for details.

---

## Español

### Descripción

Pipeline de regresión end-to-end para predecir la resistencia a la compresión del cemento según su composición química, sin necesidad de esperar 28 días para la medición física.

- **Problema**: dado que conocemos la composición química de una muestra de cemento (clínker, yeso, caliza, puzolana, etc.), ¿podemos predecir su resistencia a la compresión a los 28 días de fraguado?
- **Enfoque**: selección de características con PCA y VarianceThreshold, benchmarking de modelos, sintonización de hiperparámetros y despliegue como API REST.
- **Datos**: 31 atributos de composición química de cemento Tipo ICo, con 4 columnas target de resistencia en PSI (1, 3, 7 y 28 días).

### Qué incluye

Un pipeline multi-notebook con el flujo completo de ML:

1. **Ingesta de datos** desde Excel (composición química de cemento Tipo ICo)
2. **EDA y análisis de correlación** — matriz de Pearson, scatter plots
3. **Reducción de dimensiones** con PCA (10 componentes → 95%+ de varianza) y VarianceThreshold
4. **Benchmarking de modelos** — SVR, KNN y Random Forest con validación cruzada de 5 folds
5. **Sintonización de hiperparámetros** con GridSearchCV sobre el mejor modelo (Random Forest)
6. **Evaluación** — MAE, RMSE, MAPE sobre un conjunto de test independiente
7. **Importancia de características** — interpretabilidad directa desde Random Forest
8. **Despliegue** como API REST con FastAPI y serialización con pickle

### Por qué este proyecto

Medir la resistencia a la compresión físicamente requiere fabricar probetas y esperar 28 días. Un modelo de ML permite estimarla desde el día 1 basándose en la composición, acelerando el control de calidad y reduciendo desperdicio.

El dataset contiene **31 atributos de composición altamente correlacionados**, lo que lo convierte en un caso ideal para explorar técnicas de selección y reducción de características. El framework es replicable con el dataset público [UCI Concrete Compressive Strength](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength).

**Relevante para tesis de ingeniería civil o materiales**: combina conocimiento de dominio (química de materiales) con ML aplicado, y la contribución es clara — interpretabilidad del modelo para que el ingeniero de planta entienda qué componentes afectan más la resistencia.

### Resultados principales

| Modelo | MAE (PSI) | RMSE (PSI) | MAPE (%) |
|---|---|---|---|
| SVR | ~420 | ~560 | ~14% |
| KNN | ~380 | ~510 | ~12% |
| Random Forest (base) | ~280 | ~370 | ~9% |
| **Random Forest (GridSearchCV)** | **~235** | **~315** | **~7.5%** |

La sintonización reduce el RMSE en ~15% respecto al baseline sin tunear. El **Clínker Conforme (%)** es la característica más importante — coherente con la química del cemento.

### Inicio rápido

```bash
git clone https://github.com/FuzzyFrogAI/ml-cement-compressive-strength.git
cd ml-cement-compressive-strength
pip install -r requirements.txt
jupyter notebook notebooks/01_eda.ipynb
```

Para lanzar la API de predicción:

```bash
uvicorn api.main:app --reload
# POST a http://localhost:8000/predecir con el JSON de composición
```

### Estructura del proyecto

```
ml-cement-compressive-strength/
├── data/
│   └── composito_cemento_tipo_ico.xlsx
├── models/
│   ├── rf_cemento.pkl
│   └── scaler_cemento.pkl
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_feature_selection.ipynb
│   ├── 03_benchmarking.ipynb
│   └── 04_deploy.ipynb
├── src/
│   ├── preprocessing.py
│   └── model.py
├── api/
│   └── main.py
└── requirements.txt
```

### Stack técnico

- `scikit-learn` — SVR, KNN, Random Forest, PCA, GridSearchCV, StandardScaler, VarianceThreshold
- `pandas` / `numpy` — manipulación de datos y cálculo numérico
- `matplotlib` — visualización de predicciones y curvas de aprendizaje
- `fastapi` + `pydantic` — despliegue como API REST
- `pickle` — serialización del modelo entrenado
- `openpyxl` — ingesta del Excel

### Recursos relacionados

- **Caso de estudio completo**: [fuzzyfrog.ai/es/ai-lab/proyectos/industria/prediccion-resistencia-cemento](https://www.fuzzyfrog.ai/es/ai-lab/proyectos/industria/prediccion-resistencia-cemento/)
- **Autor**: [FuzzyFrog AI](https://fuzzyfrog.ai)

### Licencia

MIT — ver [LICENSE](LICENSE) para más detalles.

---

## Topics

`machine-learning` `regression` `random-forest` `scikit-learn` `pca` `feature-selection` `cement` `compressive-strength` `materials-engineering` `fastapi` `python` `jupyter-notebook`
