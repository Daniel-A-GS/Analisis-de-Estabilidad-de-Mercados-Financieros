# Análisis de Estabilidad del S&P 100 mediante Redes Multicapa y Causalidad de Granger

## Resumen Ejecutivo

Las redes complejas permiten realizar obtener insights adicionales a los análisis estándares con redes de tiempo, pues permite determinar interacciones complejas entre acciones de un mercado financiero. Este proyecto de análisis cuantitativo identifica los pilares estructurales, los puntos de fragilidad y los de mayor influencia dentro del índice S&P 100 mediante un enfoque innovador que combina **teoría series de tiempo financieras** y **teoría de redes complejas**. Logramos identificar:

*   **Nodos Portero (`Gatekeepers`)**: Activos clave que actúan como amortiguadores, potencialmente capaces de detener el fecto dominó en caídas abruptas del mercado.
*   **Nodos `Influencers`**: Acciones cuyo precio dirige el comportamiento del resto del mercado, siendo los principales propagadores de información.
*   **Nodos `Vulnerables`**: Compañias cuyo precio no está determinado por sus fundamentos, sino por la dinámica de la red, haciéndolas susceptibles a contagio financiero por los demás nodos.

El análisis se realiza de forma trimestral (5 redes en total), permitiendo observar la evolución de estas propiedades en el tiempo (2023-2024).

---

## Estructura del Proyecto

El flujo de trabajo está dividido en dos notebooks principales, asegurando un proceso claro y reproducible:

1.  **`Limpieza_Manipulacion_Datos.ipynb`**
    *   **Descarga de Datos:** Utiliza la API de `yfinance` para obtener los precios históricos de los componentes del S&P 100.
    *   **Preprocesamiento y Limpieza:** Manejo de valores nulos, ajuste de splits/dividendos, y alineación de las series temporales.
    *   **Cálculo de Retornos:** Transformación de precios a series de retornos logarítmicos para el análisis.

2.  **`Construccion_Red.ipynb`**
    *   **Análisis de Series de Tiempo:** Aplicación del **Test de Dickey-Fuller Aumentado (ADF)** para verificar la estacionariedad de las series, un supuesto crítico para el siguiente paso.
    *   **Test de Causalidad de Granger:** Se implementa para cada par de activos (`i -> j`). Se establece un enlace dirigido si se puede demostrar que los retornos de `i` causan en el sentido de Granger a los retornos de `j` con significancia estadística.
    *   **Construcción de la Red Multicapa:** Creación de una red multicapa (5 capas) dirigidas (una por trimestre) usando `NetworkX`, donde los nodos son las acciones y los enlaces representan relaciones de causalidad de Granger.
    *   **Análisis y Métricas de Red:** Cálculo de centralidades (Grado de Entrada/Salida, Betweenness, Cercanía) para identificar los roles clave en la red.
    *   **Visualización:** Generación de grafos estáticos con `matplotlib` y `NetworkX` para comunicar visualmente la estructura de la red y los nodos más importantes.

3. **`Research_Paper.pdf`**
    * Desarrollo detallado de la teoría de redes complejas y las pruebas de series de tiempo utilizadas en el proyecto
    * Explicación minuciosa de la metodología implementada en el proyecto
    * Interpretación Contextual de las acciones presentes en cada rol

---

## Hallazgos Clave e Insights

El análisis permite cuantificar y visualizar conceptos abstractos del mercado:

| Rol | Métrica de Identificación | Interpretación Financiera |
| :--- | :--- | :--- |
| **Nodo Portero** | Alta **Centralidad de Intermediación (`Betweenness`)** | Actúa como un "cuello de botella" a la hora de crisis financieras. Su estabilidad es crucial para la integridad de toda la red. |
| **Nodo Influencer** | Alto **Grado de Salida (`Out-Degree`)** | Sus movimientos de precio **causan** (en el sentido de Granger) los movimientos de muchos otros activos. Son líderes del mercado. |
| **Nodo Vulnerable** | Alto **Grado de Entrada (`In-Degree`)** y Baja Centralidad de Intermediación | Su precio es **causado** por muchos otros. Son "seguidores" susceptibles a volatilidad externa. |

*   **Evolución Temporal:** El análisis trimestral revela cómo estos roles cambian con el tiempo, reflejando shifts en el sentimiento del mercado, sectores en auge/caída, y el impacto de eventos macroeconómicos.
*   **Robustez Sistémica:** La identificación de `Porteros` es vital para la gestión de riesgo, ya que su quiebra podría fragmentar la red y amplificar una crisis.

---

## Stack Tecnológico

*   **Lenguaje:** `Python 3`
*   **Descarga de Datos:** API de `yfinance`
*   **Manipulación y Análisis de Datos:** `pandas`, `numpy`
*   **Pruebas de Series de Tiempo:** `statsmodels` (para ADF y Granger Causality)
*   **Construcción y Análisis de Redes:** `networkx`
*   **Visualización:** `matplotlib`, `seaborn`

---
**Nota:** Este proyecto fue desarrollado como parte de un seminario académico y es de carácter demostrativo. Los hallazgos no constituyen una recomendación de inversión.
