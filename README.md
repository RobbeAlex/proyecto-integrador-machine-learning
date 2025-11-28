# 📈 Intelligent Demand Planner: Sistema de Pronóstico de Demanda

![Python](https://img.shields.io/badge/Python-3.11%20%7C%203.12-blue)
![Streamlit](https://img.shields.io/badge/Framework-Streamlit-red)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status](https://img.shields.io/badge/Status-Completado-success)

Este proyecto es una solución integral de **Machine Learning** diseñada para optimizar la gestión de inventarios y planeación de demanda en un entorno B2B. Utiliza un enfoque de **Ensamble (Híbrido)** que combina la robustez estadística con algoritmos de aprendizaje automático moderno.

---

## 🚀 Características Principales

* **Segmentación Inteligente:** Uso de **PCA** y **K-Means Clustering** para agrupar productos y clientes según su comportamiento de compra (Ley de Pareto).
* **Pronóstico de Ensamble:** Meta-modelo que integra:
    * **SARIMA:** Para capturar estacionalidad lineal y autocorrelación.
    * **Prophet:** Para manejo de tendencias no lineales y días festivos.
    * **Random Forest:** Para capturar relaciones complejas en los datos.
* **Arquitectura Escalable:** Base de datos **SQLite** integrada para la persistencia de datos históricos y predicciones.
* **Dashboard Interactivo:** Interfaz web desarrollada en **Streamlit** que permite:
    * Filtrado por Cliente y Producto (Top 10).
    * Visualización de series de tiempo interactivas (Plotly).
    * Intervalos de confianza para la toma de decisiones bajo incertidumbre.

## 🛠️ Tecnologías Utilizadas

* **Lenguaje:** Python 3.11 / 3.12
* **Procesamiento de Datos:** Pandas, NumPy, OpenPyXL.
* **Machine Learning:** Scikit-learn, Statsmodels, Prophet.
* **Visualización:** Plotly, Matplotlib, Seaborn.
* **Aplicación Web:** Streamlit.

## 📊 Metodología enfocada en Machine Learning e Inferencia Bayesiana
El flujo de trabajo sigue el estándar de Ciencia de Datos:
1. ETL (Extracción y Limpieza): Procesamiento del dataset de facturación, limpieza de nulos y conversión a formato transaccional.
2. EDA (Análisis Exploratorio): Descomposición estacional para entender tendencias, ciclos de venta y anomalías.
3. Modelado: Entrenamiento de modelos individuales y combinación mediante promedio ponderado (Ensamble).
4. Evaluación: Uso de RMSE para medir el desempeño fuera de la muestra.
5. Despliegue: Implementación del Dashboard para consumo final del usuario de negocio.

## ⚙️ Metodología y Fundamentación enfocada en Matemáticas
La predicción final $\hat{y}_{t}$ se define como el promedio aritmético de tres predictores base, aprovechando el **Teorema del Ensamble** para reducir la varianza del error total.

$$
\hat{y}_{t} = \frac{1}{3}(\hat{y}_{RF}(t) + \hat{y}_{SARIMA}(t) + \hat{y}_{Prophet}(t))
$$

### Componentes del Modelo:
1.  **Random Forest Regressor (Componente No Paramétrico):**
    * Utiliza 100 estimadores y una profundidad máxima de 8.
    * Minimiza el MSE reduciendo la varianza mediante la segmentación del espacio de decisión.
    * Fórmula aproximada:
      
$$
{\hat{y}_{RF}}=\frac{1}{M}\sum_{m=1}^{M}{h_m(x)}
$$
      
2.  **SARIMA (Componente Estocástico Lineal):**
    * Configuración: Orden (1,1,1) con estacionalidad (1,1,1,12).
    * Optimizado mediante Estimación de Máxima Verosimilitud (MLE).
    * Ecuación general:
      
$$
\Phi_{P}(B)\Phi_{P}(B^{s})(1-B)^{d}(1-B^{s})^{D}y_{t} = \theta_{q}(B)\Theta_{Q}(B^{s})\epsilon_{t}
$$

   Donde B es el operador de retardo, y $\phi,\Phi,\theta,\ \Theta$ son los polinomios de parámetros autorregresivos y de media móvil

3.  **Prophet (Componente Aditivo Generalizado):**
    * Descompone la serie en tendencia, estacionalidad (Fourier) y efectos de días festivos.
    * Modelo:
      
$$
y(t) = g(t) + s(t) + h(t) + \epsilon_{t}
$$

## 🚀 Resultados y Evaluación
La evaluación se realizó utilizando la **Raíz del Error Cuadrático Medio (RMSE)**. Aunque SARIMA obtuvo el menor error individual, se seleccionó el Ensamble por su estabilidad y robustez estocástica ante la volatilidad del mercado.

| Modelo | RMSE | Observación |
| :--- | :--- | :--- |
| **SARIMA** | 119,770.02 | Mejor desempeño individual. |
| **Ensemble (Promedio)** | **127,591.65** | **Modelo Seleccionado (Mejor balance Sesgo-Varianza)**. |
| **Random Forest** | 148,898.06 | Alta varianza. |
| **Prophet** | 156,091.13 | Alto sesgo (suavizado excesivo). |

El modelo final entrega rangos de certeza (`Pronostico_Min` y `Pronostico_Max`) cruciales para la gestión de stock de seguridad.

## ⚙️ Instalación y Uso

Sigue estos pasos para ejecutar el proyecto en tu entorno local.

**1. Prerrequisitos**
Asegúrate de tener instalado Python 3.11 o 3.12.

⚠️ Nota Importante: No usar Python 3.14 (versión experimental) ya que es incompatible con librerías críticas como pyarrow.

**2. Clonar el repositorio**
Bash
```code
git clone [https://github.com/tu-usuario/demand-planner-collins.git](https://github.com/tu-usuario/demand-planner-collins.git)
cd demand-planner-collins
```
**3. Configurar el Entorno Virtual**
Es indispensable aislar las dependencias del proyecto para evitar conflictos.
En Windows:
PowerShell
```code
# Crear el entorno
python -m venv .venv

# Activar el entorno
.\.venv\Scripts\Activate
```
En Mac/Linux:
Bash
```code
python3 -m venv .venv
source .venv/bin/activate
```
4. Instalar Dependencias
Una vez activado el entorno, instala las librerías necesarias:
Bash
```code
pip install -r requirements.txt
```
5. Ejecutar la Aplicación
Hemos incluido un script automático para lanzar el dashboard correctamente:
Bash
```code
python run.py
```
El navegador se abrirá automáticamente en http://localhost:8501

## 📂 Estructura del Proyecto

```text
├── data/                   # Archivos CSV de entrada y base de datos SQLite generada
├── notebooks/              # Jupyter Notebook con el EDA y entrenamiento del modelo
├── src/                    # Código fuente de la aplicación
│   ├── app_dashboard.py    # Interfaz gráfica y lógica de visualización
│   └── db_manager.py       # Módulo de conexión y gestión de base de datos
├── run.py                  # Script lanzador principal (Entry Point)
├── requirements.txt        # Lista de dependencias y librerías
└── README.md               # Documentación del proyecto
```
