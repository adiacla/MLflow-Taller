# Taller MLOps · ISO/IEC 42001 + CRISP-ML(Q) — Calidad del vino tinto

Cuaderno en español, ejecutable **en local con WSL2 (Ubuntu 24.04) + VS Code**, que convierte el clásico *Red Wine Quality Model* en un flujo de trabajo de MLOps con trazabilidad:
**Git** (código y parámetros) · **DVC** (datos y pipeline) · **MLflow** (experimentos y modelo) · **evidencias JSON** (documentación para gobernanza).

## Inicio rápido

Después de instalar WSL2 + Ubuntu (ver `guia.md`):

```bash
cd ~/taller-mlops-vino
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name taller-vino --display-name "Python (taller-vino)"
mkdir -p data/raw
# Coloca winequality-red.csv en data/raw/ (si hay Internet, el cuaderno lo descarga de la UCI)
code .
```

En VS Code, abre `notebooks/Taller_MLOps_Calidad_Vino_ISO42001.ipynb`, selecciona el kernel `.venv` y ejecuta las celdas en orden.

Para ejecutar el pipeline completo desde la terminal:

```bash
dvc repro
```

## Contenido

| Ruta | Descripción |
|---|---|
| `notebooks/Taller_MLOps_Calidad_Vino_ISO42001.ipynb` | Taller completo en 8 secciones, con fases CRISP-ML(Q) 1–6 |
| `params.yaml` | Parámetros del experimento (los lee el notebook) |
| `dvc.yaml` | Pipeline de una etapa (`entrenar`) que ejecuta el notebook |
| `requirements.txt` | Dependencias con versiones probadas |
| `guia.md` | Instalación de WSL2, VS Code y el entorno, paso a paso |
| `data/raw/` | Datos crudos (no se suben a Git; se versionan con DVC) |
| `models/` | Modelo final serializado (`modelo_vino.joblib`) |
| `governance/` | Evidencia JSON generada por el notebook (datos, parámetros, versiones, métricas) |

## Flujo de versiones del taller

```
v1-con-duplicados ─► v2-sin-duplicados ─► v3-final
   (eliminar_duplicados: false)   (eliminar_duplicados: true)   (evidencias generadas)
```

Cambiar `eliminar_duplicados` en `params.yaml` y ejecutar `dvc repro` permite comparar las métricas de v1 y v2.

## Alcance

Este repositorio no incluye Hyperopt, Elasticsearch ni Kibana: se retiraron porque el notebook no los usa. MLflow guarda las corridas en `mlflow.db` (SQLite) si no hay servidor activo; con un servidor en `http://localhost:5000` las guarda allí.

> Material didáctico. Los controles de ISO/IEC 42001 se citan como referencia; verifícalos contra el texto vigente de la norma.
