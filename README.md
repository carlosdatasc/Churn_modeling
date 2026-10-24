# Predicción de Abandono de Clientes (Customer Churn)

Proyecto de ciencia de datos enfocado en predecir la fuga de clientes (*churn*) utilizando modelos de Machine Learning avanzados, priorizando la maximización del *Recall* para minimizar los costos de negocio asociados a la pérdida de usuarios.

---

## Contexto de Negocio 
En industrias de servicios por suscripción, la retención de clientes es fundamental para la rentabilidad. Sin embargo, los datasets presentan un desbalance natural de clases (la gran mayoría de los clientes permanecen y una minoría cancela). 
* **El desafío:** Un modelo tradicional basado en *Accuracy* (exactitud) es engañoso. El objetivo crítico es **maximizar el Recall** para detectar la mayor cantidad de clientes en riesgo de abandono (evitando falsos negativos costosos), manteniendo un equilibrio saludable mediante el **F1-Score**.

---

## Metodología y Pipeline del Proyecto

1. **Preprocesamiento y Limpieza:**
   * Escalado de variables numéricas (`StandardScaler`).
   * Codificación de variables categóricas (`One-Hot Encoding`).
   * Manejo adecuado de la variable objetivo (`Churn`: 'Yes' / 'No').
2. **Modelado Baseline:**
   * Evaluación inicial con **Regresión Logística** (alcanzando un *Accuracy* estable pero limitado de ~77% y baja capacidad predictiva para el fenómeno).
3. **Modelado Avanzado y Control de Overfitting:**
   * Implementación de un **Random Forest Classifier**.
   * Detección inicial de sobreajuste severo (*Recall* en train de 99.9% vs. test de 82.9%).
4. **Optimización de Hiperparámetros:**
   * Uso de `GridSearchCV` con validación cruzada (`cv=5`) optimizado explícitamente para **`scoring='recall'`**.
   * Hiperparámetros óptimos encontrados: `max_depth=10`, `n_estimators=50`, `min_samples_split=2`, `min_samples_leaf=1`, `max_features='sqrt'`.

---

## Resultados y Métricas del Modelo Optimizado

Tras el ajuste de hiperparámetros, se eliminó por completo el sobreajuste, logrando un rendimiento generalizable y robusto:

* **Recall en Train:** 89.32%
* **Recall en Test:** 89.51% *(Brecha: -0.19%, sin overfitting)*
* **F1-Score en Train:** 78.36%
* **F1-Score en Test:** 78.64%

El modelo final identifica correctamente a **casis 9 de cada 10 clientes** que efectivamente van a cancelar su servicio.

---

## Insights Clave

A través de la extracción de *Feature Importance* del Random Forest optimizado, se identificó el factor determinante en la fuga de clientes:

1. **El Tipo de Contrato (`Contract`):** Concentra aproximadamente el **89.1% del peso total** en las decisiones del modelo. Los contratos mensuales (*month-to-month*) presentan una tasa de abandono drásticamente superior en comparación con los contratos anuales o bianuales.
2. **Factores Secundarios:** Variables como `MonthlyCharges`, `TotalCharges` y `tenure` (antigüedad) influyen en menor medida, respaldando el comportamiento financiero del usuario.

### Recomendación Estratégica:
El problema principal de abandono no radica en la calidad aislada del servicio, sino en la modalidad de contratación. Se recomienda a la empresa enfocar esfuerzos comerciales en **estrategias de migración de contratos mensuales a anuales** mediante incentivos de retención.

---

