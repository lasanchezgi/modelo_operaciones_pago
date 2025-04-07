# 💼 | Modelo de propensión a la aceptación de opciones de pago

Este proyecto desarrolla una solución analítica E2E que permite pronosticar si un cliente en mora aceptará alguna de las opciones de pago preaprobadas ofrecidas por Bancolombia, dentro de una ventana de un mes.

El modelo propuesto busca ser integrado como una nueva variable estratégica dentro del sistema de priorización de cartera, aumentando la eficiencia de la gestión y ayudando a disminuir la cartera vencida.

---

## 🏦 | Contexto del negocio

Bancolombia ofrece diversas **opciones de pago** como alivio financiero para clientes en mora (e.g., ampliación de plazo, reducción de cuota, renegociación de tasa). Estas se manejan como productos preaprobados, pero su aceptación depende de múltiples factores del cliente y la obligación.

La predicción de esta aceptación con antelación permite una gestión más proactiva, focalizada y eficiente.

---

## 🎯 | Objetivo del proyecto

Diseñar, entrenar y desplegar un modelo de clasificación binaria que determine si una obligación en mora aceptará una opción de pago en el siguiente mes de gestión.

**Variable objetivo (𝑌):**

> - `1`: El cliente aceptó una opción de pago.
> - `0`: El cliente no aceptó una opción de pago.

---

## 🗂️ | Estructura del proyecto

```plaintext
modelo_operaciones_pago/
│
├── README.md                <- Documento actual
│
├── datos/
│   ├── raw/                <- Archivos originales entregados en la prueba
│   ├── interim/            <- Datos listos para modelado y predicción 
│   └── processed/          <- Datos limpios y unificados
│
├── modelos/                 <- Modelos entrenados y serializados (.pkl)
│
├── notebooks/
│   ├── 01_pre_procesamiento_datos.ipynb
│   ├── 02_procesamiento_datos.ipynb
│   ├── 03_entrenamiento_modelo.ipynb
│   └── 04_generacion_predicciones.ipynb
│
├── referencias/             <- Diccionarios de datos y documentación externa
│
├── requerimientos.txt       <- Dependencias para reproducir el entorno
└── pyproject.toml           <- Configuración del proyecto
```

---

## 🔎 Flujo de Trabajo

### 🧼 1. Pre-procesamiento de datos

- Carga y exploración inicial de todos los archivos `.csv`.
- Validación de fechas de corte (solo 202308 a 202312).
- Unión de fuentes para crear una **sábana consolidada**.
- Identificación y tratamiento de **valores atípicos** y **valores nulos**.
- Exportación de la sábana limpia (`processed/`).

### ⚙️ 2. Procesamiento de variables

- Análisis de distribución y utilidad de cada variable.
- Eliminación de columnas redundantes o poco informativas.
- Conversión de variables categóricas con `get_dummies`.
- Escalado de variables numéricas.
- Exportación final de datos procesados (`procesados/`).

### 🧠 3. Entrenamiento del modelo

- Separación en `X` y `y`.
- Selección de variables con `SelectKBest`.
- Comparación de modelos: Regresión Logística, Random Forest, XGBoost.
- Validación cruzada, métricas de rendimiento, y evaluación con:
  - Matriz de confusión
  - Curva ROC
  - Análisis por deciles
- Modelo final seleccionado: **Regresión Logística** por buen desempeño y simplicidad.
- Serialización del modelo y features seleccionadas (`modelos/`).

### 📤 4. Generación de predicciones

- Importación de modelo y columnas relevantes.
- Aplicación sobre nuevos datos (`oot`).
- Generación del archivo `resultado_prueba.csv` en el formato exigido:
  
```csv
id,var_rpta_alt
123456#987#0001,1
123456#987#0002,0
```

---

## 📈 | Resultados del modelo

- **Modelo seleccionado**: Regresión Logística
- **Métrica principal**: F1 Score
- **Evaluación final**: [incluir valor obtenido, por ejemplo: `F1 Score = 0.76`]
- Se logró un buen balance entre precisión y sensibilidad, con interpretabilidad alta.

---

## 🛠️ | Requisitos del entorno

Crear un entorno virtual y ejecutar:

```bash
pip install -r requirements.txt
```

Versiones clave:

- Python 3.10+
- pandas, numpy, scikit-learn, xgboost, seaborn, matplotlib, etc.

---

## 💡 | Consideraciones finales y futuras mejoras

- Despliegue propuesto mediante una API REST con FastAPI o Flask.
- Automatización y monitoreo del modelo en producción.
- Posibilidad de actualizar el modelo mensualmente con nuevas cohortes.

---

## 💡 Autor y agradecimientos

[Laura Sánchez Giraldo](mailto:laurasanchezgiraldo@outlook.es)  

Este proyecto fue desarrollado como parte de una prueba técnica para **Bancolombia**, destacando una solución sólida, transparente y modular.  

> Con ayuda de GitHub Copilot, ChatGPT, Claude y Gemini, como copilotos técnicos en todo el camino 🚀
