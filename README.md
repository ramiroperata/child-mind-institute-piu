
# Child Mind Institute - Competencia sobre Uso Problemático de Internet

Bienvenido al repositorio de mi participación en la competencia **Child Mind Institute - Problematic Internet Use** en Kaggle. Este README proporciona una visión general del proyecto, incluyendo el análisis de datos, preprocesamiento, modelos utilizados, y la métrica de evaluación. La idea es ofrecer un documento claro y educativo para comprender cada parte del proceso.

---

## Descripción del Proyecto

El **Uso Problemático de Internet (PIU)** es una preocupación creciente, especialmente entre adolescentes. El objetivo de esta competencia es predecir los puntajes de severidad de PIU basados en datos de encuestas. El conjunto de datos incluye:

- **Respuestas a encuestas**: Preguntas que evalúan comportamientos y tendencias.
- **Características de series temporales**: Señales derivadas que capturan patrones en el tiempo.

Este proyecto utiliza técnicas avanzadas de preprocesamiento, ingeniería de características y modelos en conjunto para lograr predicciones robustas. Entre las metodologías destacadas se encuentran el uso de un **autoencoder para series temporales** y la evaluación mediante la métrica **Quadratic Weighted Kappa (QWK)**.

---

## Tabla de Contenidos

1. [Análisis Exploratorio de Datos (EDA)](#análisis-exploratorio-de-datos)
2. [Preprocesamiento](#preprocesamiento)
3. [Autoencoder para Series Temporales](#autoencoder-para-series-temporales)
4. [Modelos y Enfoque de Modelado](#modelos-y-enfoque-de-modelado)
5. [Métrica de Evaluación](#métrica-de-evaluación)
6. [Resultados](#resultados)

---

## Análisis Exploratorio de Datos

Los pasos iniciales incluyeron:

- **Análisis de distribuciones**: Examinamos las distribuciones de las variables objetivo y predictoras.
- **Gestión de datos faltantes**: Identificamos valores nulos y los imputamos cuando fue necesario.
- **Correlación de características**: Analizamos las correlaciones entre las variables predictoras y el objetivo para identificar los principales impulsores del PIU.

Visualizaciones y resúmenes estadísticos guiaron estas investigaciones, proporcionando una base sólida para el preprocesamiento y modelado.

---

## Preprocesamiento

Pasos clave del preprocesamiento:

1. **Imputación**: Se utilizó un `SimpleImputer` con la estrategia de mediana para manejar valores faltantes.
2. **Escalado**: Se aplicó una normalización estándar para garantizar compatibilidad entre las características.
3. **Ingeniería de características**: Se extrajeron nuevos atributos, como resúmenes estadísticos y tendencias de los datos de series temporales.

Estos pasos garantizaron un conjunto de datos limpio, normalizado y enriquecido para el modelado posterior.

---

## Autoencoder para Series Temporales

Un **autoencoder** fue empleado para comprimir y reconstruir las características de las series temporales. Este modelo es especialmente útil para reducir la dimensionalidad de datos complejos mientras se preserva la información relevante.

### Fundamentos Teóricos

Un autoencoder es una red neuronal que consta de dos componentes principales:

1. **Codificador (Encoder)**: Transforma los datos de entrada en una representación de menor dimensión (espacio latente).
2. **Decodificador (Decoder)**: Reconstruye los datos originales a partir de la representación comprimida.

![Screenshot-2024-05-15-022904](https://github.com/user-attachments/assets/79514d05-034e-4f34-886a-fa8ef958ebfb)

El objetivo es minimizar la **pérdida de reconstrucción**, como el error cuadrático medio (MSE), para aprender una representación compacta pero informativa de los datos.

En este proyecto:

- El autoencoder fue entrenado con la función de pérdida MSE.
- Las características codificadas fueron utilizadas como entradas para los modelos de predicción, logrando una reducción de dimensionalidad efectiva, imputando valores faltantes y preservando patrones clave.

---

## Modelos y Enfoque de Modelado

Se entrenó un conjunto diverso de modelos para predecir los puntajes de PIU, los que mejores resultados dieron fueron:

1. **CatBoostRegressor**: El cual usa un ordered boosting que crea múltiples permutaciones de los datos y entrena el modelo en un subconjunto mientras calcula los residuos en otro. Esta técnica ayuda a prevenir data leakage y sobreajustes.
2. **LightGBM**: Utiliza un algoritmo basado en histogramas, es decir, agrupa valores de características continuas en contenedores discretos que agilizan el procedimiento de entrenamiento. Esto es muy conveniente debido al grado de experimentacion que fue necesario.
3. **XGBoost**: Algoritmo muy flexible y potente.
4. **Regresor por votación (Voting Regressor)**: Combinación de predicciones de los modelos individuales para mejorar el desempeño.

Cada modelo fue optimizado mediante ajuste de hiperparámetros y evaluado en un conjunto de validación.

---

## Métrica de Evaluación

La métrica principal usada fue el **Quadratic Weighted Kappa (QWK)**, que mide el grado de concordancia entre dos conjuntos de clasificaciones ordinales, teniendo en cuenta la concordancia aleatoria.

### Fórmula

El QWK se calcula como:

$k=1-\frac{\sum_{i,j}w_{i,j}o_{i,j}}{\sum_{i,j}w_{i,j}e_{i,j}}\$

Donde:
- $(\ o_{i,j})\$ : Matriz de concordancia observada.
- $(\ e_{i,j})\$ : Matriz de concordancia esperada bajo el azar.
- $(\ w_{i,j})\$ : Pesos que penalizan los desacuerdos más grandes.

### Importancia del QWK

El QWK es particularmente útil para datos ordinales, donde la magnitud del desacuerdo importa. Por ejemplo, predecir un puntaje de 3 cuando el verdadero es 4 es menos severo que predecir un 1.

En este proyecto, el QWK se utilizó para:

1. Evaluar el desempeño de los modelos en los conjuntos de validación y prueba.
2. Optimizar los umbrales de redondeo para convertir predicciones continuas en categorías ordinales.

---

## Resultados
- **QWK en validación**: 0.463

El ensamblado final logró un desempeño competitivo, demostrando la efectividad del enfoque combinado.

---

## Estructura del Repositorio

```
root
├── data/           # Datos crudos
├── notebook/       # Notebooks de Jupyter para análisis y modelado
├── modelos/        # Modelos guardados
└── README.md       # Documentación del proyecto
```

---

## Futuro Trabajo

- Incorporar técnicas adicionales de ingeniería de características.
- Experimentar con arquitecturas alternativas para el autoencoder.
- Explorar métodos avanzados de ensamblado.

---

Gracias por explorar este repositorio. ¡No dudes en contactarme con preguntas o sugerencias!
