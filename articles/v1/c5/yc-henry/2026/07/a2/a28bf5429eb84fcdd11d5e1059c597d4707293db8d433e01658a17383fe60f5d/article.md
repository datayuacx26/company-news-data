---
schema_version: "1.0.0"
document_id: "a28bf5429eb84fcdd11d5e1059c597d4707293db8d433e01658a17383fe60f5d"
company_key: "yc-henry"
company: "HENRY"
source_id: "yc-henry-rss-d34ee96ffff8"
canonical_url: "https://blog.soyhenry.com/tu-primer-modelo-predictivo-con-python-y-scikit-learn-tutorial-para-principiantes/"
published_at: "2026-07-29T12:00:00+00:00"
first_seen_at: "2026-07-29T12:58:55.912859+00:00"
fetched_at: "2026-07-29T12:58:57.029746+00:00"
content_hash: "sha256:3dc2431b33d052bfaf3085b6060f27df167f6a93bece04e40d7298f2393b8f40"
---

# Tu primer modelo predictivo con Python y scikit-learn: tutorial para principiantes

Hay un momento muy concreto en el que alguien deja de "estudiar" Machine Learning y empieza a hacerlo: cuando entrena su primer modelo y lo ve predecir algo, aunque sea sobre datos simples. Ese momento no requiere semanas de matemática avanzada. Requiere Python, una librería llamada scikit-learn y entender un flujo de cinco pasos que se repite en la enorme mayoría de los proyectos de Machine Learning.


**En esta nota te explicamos, paso a paso, cómo construir tu primer modelo desde cero.**


## **Qué es un modelo predictivo**


Un **modelo predictivo** es un **programa que aprende un patrón** a partir de datos históricos y lo usa para **estimar un valor sobre datos nuevos** que nunca vio. Si le muestras cientos de departamentos con sus metros cuadrados y su precio de alquiler, el modelo aprende la relación entre ambas variables, y después puedes darle los metros cuadrados de un departamento nuevo para que estime su precio.


Ese "aprender un patrón" se llama entrenamiento, y **scikit-learn** es la librería de Python más usada para hacerlo: ofrece, con muy poco código, las herramientas para dividir datos, entrenar un modelo y medir qué tan bien predice.


## **Qué necesitas antes de empezar**


Para este tutorial necesitas:


- Python instalado (versión 3.9 o superior)
- Dos librerías: **pandas** , para manejar los datos en tablas, y **scikit-learn** , para el modelo. Ambas se instalan con **pip install pandas scikit-learn** desde la terminal.
- También conviene trabajar en un **Jupyter Notebook** , que te permite ejecutar el código de a partes y ver los resultados en el momento, aunque cualquier editor con Python funciona.


No necesitas una computadora potente ni datos de gran escala: el ejemplo de esta guía corre en cualquier laptop en segundos, con un archivo de datos de unas pocas columnas.


*📎 Antes de modelar, todo proyecto real empieza por limpiar y entender los datos; si quieres ese paso previo, esta guía sobre*[análisis predictivo y limpieza de datos](https://blog.soyhenry.com/analisis-predictivo-101-como-limpiar-y-visualizar-datos-usando-inteligencia-artificial-2/) *lo cubre en detalle.*


## **Cómo construir tu primer modelo predictivo, paso a paso**


Vamos a construir un modelo que predice el precio de alquiler de un departamento a partir de sus metros cuadrados. El ejemplo es simple porque el objetivo es que entiendas el flujo completo, no que memorices sintaxis.


### **1️⃣ Prepara tus datos**


Cargas tu archivo de datos (por ejemplo, un CSV con columnas de metros cuadrados y precio) en una tabla usando **pandas** con la función **read_csv** . Antes de seguir, revisa los valores faltantes por columna con **isnull().sum()** y confirma que las columnas numéricas tengan tipo numérico y no texto (puedes verlo con **dtypes** ); un modelo no puede entrenarse sobre datos sucios.


### **2️⃣ Separa qué predice y qué quieres predecir**


Todo proyecto de Machine Learning supervisado divide sus datos en dos partes: las variables predictoras, que por convención se llaman **X** (en este caso, los metros cuadrados), y la variable que quieres predecir, que se llama **y** (el precio). Un tropiezo común acá: scikit-learn espera que **X** sea una tabla, aunque tenga una sola columna, mientras que **y** puede ser una columna simple. Seleccionar mal esa forma es la causa más frecuente de un error confuso en el primer intento.


### **3️⃣ Divide en datos de entrenamiento y de prueba**


Acá entra la función más importante de scikit-learn para principiantes: **train_test_split** , del módulo **model_selection** . Le pasas **X** e **y** , y devuelve cuatro partes: **X_train** , **X_test** , **y_train** e **y_test** . Reserva una porción de tus datos —típicamente un 20%, con el parámetro **test_size=0.2** — como conjunto de prueba, que el modelo no va a ver durante el entrenamiento. Fijá también **random_state** (cualquier número entero, por ejemplo 42) para que la división sea siempre la misma cada vez que corras el código; sin eso, cada ejecución mezcla los datos distinto y los resultados varían de una corrida a otra. La razón de separar entrenamiento y prueba es simple: si evalúas al modelo con los mismos datos con los que aprendió, no sabes si realmente entendió el patrón o si solo los memorizó.


### **4️⃣ Entrena el modelo**


Eliges un algoritmo —para este caso, **LinearRegression** , del módulo **linear_model** , que busca la línea que mejor explica la relación entre metros cuadrados y precio— y lo entrenas con el método **fit()** , pasándole los datos de entrenamiento ( **X_train** e **y_train** ). Ese único paso es, literalmente, el momento en que el modelo aprende.


### **5️⃣ Evalúa qué tan bien predice**


Con el modelo ya entrenado, generas predicciones sobre los datos de prueba con **predict()** y las comparas contra los valores reales con las funciones de evaluación del módulo **sklearn.metrics** . Para un problema de este tipo (predecir un número), las más usadas son **mean_absolute_error** —cuánto se equivoca el modelo, en promedio, en las mismas unidades que el precio— y **r2_score** , que indica qué proporción de la variación del precio explica el modelo, en una escala de 0 a 1. Como referencia orientativa para un primer modelo simple: un R² por debajo de 0.5 sugiere que falta información relevante (más variables, más datos), mientras que uno por encima de 0.7 ya indica que el modelo capturó una relación bastante sólida.


*Este flujo de cinco pasos es la base de casi cualquier proyecto de Machine Learning, y es exactamente lo que se practica desde el primer proyecto en la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=tuPrimerModeloPredictivoPythonScikitLearn) *, con mentores que trabajan en la industria.* 🚀


## **Qué hacer si tu modelo predice mal**


Es normal que tu primer modelo no prediga demasiado bien, y no significa que hiciste algo mal: significa que hay margen para mejorar.


Antes de asumir que el algoritmo está roto, revisa lo siguiente:


#### ⚠️ **Pocos datos o poco representativos.**


Un modelo entrenado con pocos ejemplos, o con datos que no reflejan la variedad real de casos, aprende un patrón débil. Más datos de calidad suele ayudar más que cambiar de algoritmo.


#### ⚠️ **Una sola variable no alcanza.**


El precio de un alquiler depende de más factores que los metros cuadrados —la ubicación, la antigüedad, los ambientes—. Sumar variables predictoras relevantes (regresión múltiple) casi siempre mejora el resultado.


#### ⚠️ **Buen resultado en entrenamiento, malo en prueba.**


Es la señal clásica de sobreajuste (overfitting): el modelo memorizó los datos de entrenamiento en lugar de aprender un patrón general. Es, justamente, la razón por la que separaste un conjunto de prueba en el paso 3.


*Aprender a diagnosticar estos problemas, y no solo a ejecutar el código, es lo que distingue a un perfil junior de Data Science. Da el paso: aplica a la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=tuPrimerModeloPredictivoPythonScikitLearn) *y desarrolla ese criterio sobre proyectos reales.* 💡


## **Qué sigue después de tu primer modelo**


Una vez que este flujo te resulta natural, el camino se abre en varias direcciones: otros algoritmos para regresión (como árboles de decisión o bosques aleatorios), problemas de clasificación en lugar de predicción de un número, y técnicas para mejorar un modelo sin simplemente sumar datos, como ajustar sus parámetros o construir mejores variables a partir de las que ya tienes.


*Ese último punto — **construir mejores variables** — tiene más impacto del que parece: en la práctica, decide más el resultado que el algoritmo elegido. 📚 Si quieres profundizar en cómo se preparan y transforman los datos antes de llegar a esta etapa, te recomendamos esta nota sobre[las herramientas del científico de datos](https://blog.soyhenry.com/las-herramientas-del-cientifico-de-datos-tecnologias-que-impulsan-el-futuro-de-la-inteligencia/) .*


## **En resumen**


- Un **modelo predictivo** aprende un patrón a partir de datos históricos para estimar valores sobre datos nuevos.
- **scikit-learn** ofrece, en pocas líneas, todas las herramientas del flujo básico de Machine Learning.
- El flujo tiene **cinco pasos** : preparar los datos, separar variables predictoras y objetivo, dividir en entrenamiento y prueba, entrenar el modelo y evaluar sus predicciones.
- **train_test_split** existe para evitar evaluar el modelo con los mismos datos con los que aprendió.
- Si el modelo predice mal, revisa **la cantidad de datos, si falta alguna variable relevante o si hay sobreajuste** , antes de cambiar de algoritmo.


## **Conclusión**


**Construir tu primer modelo predictivo** no requiere dominar la matemática detrás de cada algoritmo: requiere **entender el flujo** —preparar los datos, separarlos, entrenar y evaluar— y **practicarlo** hasta que se vuelva automático. Una vez que ese patrón queda incorporado, se repite, con variaciones, en prácticamente cualquier proyecto de Machine Learning que construyas después. Ese es el verdadero punto de partida de un perfil de Data Science: no memorizar sintaxis, sino entender qué está pasando en cada paso.


*Si quieres seguir construyendo modelos sobre datos reales, con acompañamiento de mentores que trabajan en la industria y proyectos que terminan en tu portfolio, la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=tuPrimerModeloPredictivoPythonScikitLearn) *está diseñada para eso. Aplica y empieza a construir tu perfil en datos desde el primer proyecto.* 🚀


## **Preguntas frecuentes**


**¿Necesito saber matemática avanzada para entrenar mi primer modelo?**


No para empezar. scikit-learn abstrae buena parte de las matemáticas detrás de cada algoritmo, así que puedes entrenar y evaluar un modelo sin conocer a fondo su fórmula. Entender los conceptos generales —qué es una variable, qué es sobreajuste— alcanza para este primer paso; la profundidad matemática se suma con la práctica.


**¿Qué diferencia hay entre un problema de regresión y uno de clasificación?**


En un problema de regresión, como el de esta nota, el modelo predice un número (un precio, una temperatura). En uno de clasificación, predice una categoría (si un cliente se da de baja o no, si un correo es spam). El flujo de trabajo es prácticamente el mismo; cambia el tipo de algoritmo y las métricas de evaluación.


**¿Por qué se separan los datos en entrenamiento y prueba?**


Para poder medir con honestidad qué tan bien generaliza el modelo. Si lo evalúas con los mismos datos con los que aprendió, puede parecer que predice perfecto simplemente porque los memorizó, y esa ilusión desaparece apenas lo enfrentas a datos que nunca vio.


**¿Cuánto tiempo lleva aprender a construir modelos predictivos con scikit-learn?**


El flujo básico —los cinco pasos de esta nota— se aprende en un par de sesiones de práctica. Lo que lleva más tiempo, y es lo que realmente forma a un Data Scientist, es desarrollar criterio: elegir bien las variables, interpretar los resultados y saber qué hacer cuando el modelo no funciona como esperabas.


### Etiquetas


- [Data Science](https://blog.soyhenry.com/tag/data-science/)
- [Cómo hacer](https://blog.soyhenry.com/tag/como-hacer/)
- [Henry](https://blog.soyhenry.com/tag/henry/)
- [Mundo Henry](https://blog.soyhenry.com/tag/mundo-henry/)
- [Trabajar en Tecnología](https://blog.soyhenry.com/tag/trabajar-en-tecnologia/)
