# TFM – Sistema de Previsión de Demanda para H&M
### Metodología CRISP-DM · Aprendizaje Automático · Python

**Trabajo de Final de Máster**  
Autora: Laura Fernández  
Máster en Data Science / Business Analytics  

---

## Descripción

Sistema integral de previsión de demanda basado en aprendizaje automático para optimizar la gestión de inventario en H&M, desarrollado siguiendo la metodología CRISP-DM. El proyecto traduce datos transaccionales de más de 31 millones de registros en recomendaciones operativas de inventario alineadas con el Reglamento (UE) 2024/1781.

**Pregunta de investigación:** ¿Cómo puede un sistema predictivo basado en aprendizaje automático optimizar dinámicamente el inventario de H&M para cumplir con el Reglamento (UE) 2024/1781 mientras se maximiza la eficiencia comercial?

---

## Estructura del proyecto

```
tfm-demand-forecasting-hm/
│
├── TFM_CRISP_DM_HM.py          # Pipeline completo CRISP-DM (Fases 1–6)
├── README.md                    # Este archivo
│
├── data/                        # Carpeta para los datasets de H&M (no incluidos)
│   ├── transactions_train.csv
│   ├── articles.csv
│   └── customers.csv
│
└── outputs/                     # Resultados generados al ejecutar el pipeline
    ├── eda_01_ventas_por_categoria.png
    ├── eda_02_evolucion_temporal.png
    ├── eda_03_estacionalidad.png
    ├── eda_04_descomposicion_stl.png
    ├── eda_05_canal_ventas.png
    ├── eda_06_perfil_demanda.png
    ├── eda_07_sell_through_seccion.png
    ├── modelo_comparativa_metricas.png
    ├── modelo_lgb_feature_importance.png
    ├── inv_01_rop_por_categoria.png
    ├── inv_02_riesgo_sobrestock.png
    ├── inv_03_ss_vs_cv.png
    └── politica_inventario_hm.csv
```

---

## Fases del pipeline CRISP-DM

| Fase | Descripción |
|------|-------------|
| **Fase 1** | Comprensión del negocio: KPIs, pregunta de investigación, marco normativo |
| **Fase 2** | ETL: carga con Polars, limpieza, agregación a granularidad artículo-semana |
| **Fase 3** | EDA: estacionalidad, sparsity, perfiles de demanda, sell-through proxy |
| **Fase 4** | Feature Engineering: lags, ventanas móviles, Google Trends, codificaciones |
| **Fase 5** | Modelado: Baseline MA-4w, SARIMA, Prophet, LightGBM, XGBoost + walk-forward CV |
| **Fase 6** | Implantación: política dinámica de inventario (ROP, SS, plan normativo) |

---

## Datos

El dataset utilizado es **H&M Personalized Fashion Recommendations**, disponible públicamente en Kaggle:

🔗 https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations

Los archivos de datos **no se incluyen en este repositorio** por su tamaño (~3.4 GB). Para ejecutar el pipeline:

1. Descarga los tres archivos CSV desde Kaggle
2. Colócalos en la carpeta `data/`
3. Ejecuta el script principal

---

## Instalación y ejecución

### Requisitos

- Python 3.9 o superior

### Instalación de dependencias

Se recomienda utilizar un entorno virtual para mantener las dependencias aisladas:

```bash
# 1. Crear el entorno virtual
python -m venv venv

# 2. Activar el entorno virtual
# En macOS/Linux:
source venv/bin/activate
# En Windows:
venv\Scripts\activate

# 3. Instalar todas las dependencias requeridas
pip install -r requirements.txt

### Ejecución

```bash
python TFM_CRISP_DM_HM.py
```
---

## KPIs del proyecto

| KPI | Objetivo |
|-----|----------|
| Reducción WAPE vs. baseline | > 10 % |
| Reducción de sobrestock al cierre de temporada | ≥ 15 % |
| Sell-through rate por categoría | > 85 % |
| Referencias con plan de acción normativo documentado | 100 % |

---

## Modelos implementados

| Modelo | Tipo | Descripción |
|--------|------|-------------|
| MA-4w | Baseline | Media móvil 4 semanas (práctica habitual del sector) |
| SARIMA | Local / estadístico | Modelo lineal para series individuales estables |
| Prophet | Local / flexible | Descomposición de tendencia, estacionalidad y festivos |
| LightGBM | Global / ML | Cross-learning sobre todas las series del catálogo |
| XGBoost | Global / ML | Alternativa robusta; validación cruzada comparativa |

La evaluación se realiza mediante **walk-forward cross-validation con 6 folds**, garantizando que el modelo no accede a datos futuros durante el entrenamiento.

---

## Marco normativo

El sistema incorpora el **Reglamento (UE) 2024/1781** del Parlamento Europeo y del Consejo, que prohíbe la destrucción de prendas y calzado no vendidos a partir de julio de 2026. La Fase 6 genera un plan de acción alternativo documentado (liquidación, donación, redistribución) para cada artículo clasificado con riesgo de sobrestock.

---

## Uso de inteligencia artificial

En el desarrollo de este proyecto se utilizaron herramientas de IA generativa (Claude, Gemini y ChatGPT) como apoyo en la escritura y depuración del código. Las decisiones metodológicas, la selección de modelos y la interpretación de resultados son íntegramente de la autora.

---

## Referencia del dataset

H&M Group. (2022). *H&M Personalized Fashion Recommendations*. Kaggle.  
https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations

---

## Licencia

Proyecto académico sin fines comerciales. Los datos de H&M pertenecen a H&M Group y están sujetos a sus términos de uso en Kaggle.
