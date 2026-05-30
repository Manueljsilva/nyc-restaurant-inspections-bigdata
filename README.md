# 🗽 Big Data Analytics: NYC Restaurant Inspections

## 📌 Descripción
Análisis de ~297,000 registros de inspecciones sanitarias del 
Departamento de Salud de Nueva York (DOHMH) usando procesamiento 
distribuido en Google Cloud Platform. El proyecto construye 10 KPIs 
sanitarios para identificar patrones de riesgo, reincidencia y 
desempeño por zona geográfica y tipo de cocina.

**Dataset:** [DOHMH NYC Restaurant Inspection Results](https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j) — NYC OpenData

---

## 🛠️ Stack Tecnológico

| Capa | Tecnología |
|------|-----------|
| Infraestructura | Google Cloud Storage + Dataproc (1 master, 2 workers n4-standard) |
| Procesamiento distribuido | Apache Spark / PySpark |
| Procesamiento paralelo | Dask, Modin (Ray) |
| Lenguaje | Python 3 |
| Formato de checkpoint | Parquet (PyArrow) |

---

## 🏗️ Arquitectura  

    NYC OpenData
            ↓
    Google Cloud Storage (raw/)
            ↓
    Dataproc Cluster
    ├── Dask    → processed/dask/   + results/kpis/dask/
    ├── Modin   → processed/modin/  +results/kpis/modin/
    └── Spark   → processed/spark/  + results/kpis/spark/

## 📊 KPIs Desarrollados

| # | KPI | Herramienta | Resultado destacado |
|---|-----|-------------|-------------------|
| 1 | Efectividad de reinspección | Spark | 30.1% de mejora promedio |
| 2 | Deterioro estacional | Spark | Staten Island Caribbean: +425% en verano |
| 3 | Reincidencia post-clausura | Dask | MEEM SPICY GROCERY: 67 violaciones post-clausura |
| 4 | Tasa de violaciones por tipo | Dask | Contaminación de alimentos: 19.4% |
| 5 | Riesgo sanitario por zona | Spark | Queens: índice 318.52 (mayor riesgo) |
| 6 | Excelencia sostenida | Spark | Solo 0.31% mantiene cero infracciones críticas |
| 7 | Tiempo de subsanación | Dask | Promedio >1000 días en casos extremos |
| 8 | Severidad de violación | Modin | Índice global: 1.55 |
| 9 | Reincidencia geográfica | Modin | ZIP 11411: 95.45% de restaurantes reincidentes |
| 10 | Presión de inspección | Modin | Bronx lidera con 3.19 inspecciones/restaurante |

---

## 🚀 Reproducción del proyecto

El pipeline fue diseñado y probado  **Google Cloud Platform**:

1. Subir el dataset al bucket en `gs://<tu-bucket>/raw/`
2. Crear un cluster Dataproc con Jupyter habilitado
   (imagen: `2.3-debian12`, 1 master n4-standard-4, 2 workers n4-standard-2)
3. Configurar Cloud NAT para acceso a PyPI desde los nodos
4. Ejecutar los notebooks en orden desde JupyterLab:
   - `notebooks/dask_pipeline.ipynb`
   - `notebooks/modin_pipeline.ipynb`  
   - `notebooks/spark_pipeline.ipynb`

---
## 💡 Hallazgos Clave

- Las **reinspecciones son efectivas** en promedio (30.1% de reducción 
  de puntaje), pero no garantizan mejora uniforme — algunos 
  establecimientos empeoran.
- **Queens concentra el mayor riesgo sanitario** combinando puntaje 
  promedio, carga crítica y reincidencia.
- Mantener **cero infracciones críticas sostenidas es extremadamente 
  raro**: solo 16 de 5,193 restaurantes evaluados lo lograron (0.31%).
- Algunos restaurantes tardan **más de 1,000 días** en pasar de 
  calificación B/C a A.