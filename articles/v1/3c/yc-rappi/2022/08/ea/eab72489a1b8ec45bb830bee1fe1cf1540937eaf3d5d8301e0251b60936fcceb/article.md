---
schema_version: "1.0.0"
document_id: "eab72489a1b8ec45bb830bee1fe1cf1540937eaf3d5d8301e0251b60936fcceb"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/b%C3%BAsqueda-eficiente-de-hiperpar%C3%A1metros-con-optuna-sklearn-para-xgboost-y-lightgbm-96805f1a48ed"
published_at: "2022-08-26T14:53:49+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-08-20T02:17:23.302365+00:00"
content_hash: "sha256:d75eabfc772df65a644ba3b2f34f25941129c1c5cb860f22923b12a47119e127"
---

# Búsqueda eficiente de hiperparámetros con Optuna + Sklearn para XGBoost y LightGBM

En el proceso de aprendizaje máquina, encontrar la mejor combinación de hiperparámetros para maximizar o minimizar una métrica objetivo como el ‘AUC’, ‘F1’, ‘Recall’, ‘Logloss’ etc, se convierte en una de las tareas principales del científico de datos, ya que, esto nos permite definir soluciones *ad-hoc* para un producto de datos con metas *especificas* de negocio, por ejemplo:


- En un modelo para fraude donde nuestro objetivo es aumentar la tasa de onboarding sin afectar a clientes buenos (maximizando ‘Precision’).
- En un modelo para riesgo crediticio en onboarding, nuestro objetivo es detectar clientes morosos donde la prioridad es la salud de la cartera (maximizando ‘Recall’).
- Ó Encontrando un equilibrio optimizando para f1, siempre evitando el sobre ajuste.


Aquí entra Optuna.


Hola, mi nombre es Rodrigo trabajo para **RappiPay** como científico de datos.
En este post, pretendo mostrarte como funciona la optimización de hiperparámetros mediante el framework de Optuna, a partir de ejemplos ‘juguete’ y ejemplos más complejos de machine learning con módulos como ‘XGBoost’ y ‘LightGBM’.


### ¿Qué es Optuna?


Optuna es un framework de código libre para automatizar el proceso de búsqueda y optimización de hiperparámetros.


¿Cómo?


Encuentra automáticamente valores óptimos de los hiperparámetros haciendo uso de distintos tipos de muestreos, algunos de ellos son:


- Random: Se muestrea en un espacio de búsqueda hasta cumplir con los criterios de paro definidos en la prueba.
- Grid search: Se define un espacio discretizado de los parámetros por prueba y se regresa el mejor al final.
- Tree-structured Parzen Estimator: En cada intento ajusta un modelo de gaussianos mixtos para arrojar los valores de los parámetros con las mejores métricas del objetivo de ensayo.
- Bayesian: Basado en modelos probabilísticos a priori para encontrar los hiperparámetros óptimos por prueba.
- Evolutionary algorithms (NSGA-II): “Nondominated Sorting Genetic Algorithm II”, algoritmo genético multiobjetivo.


Las principales ventajas que encuentro al utilizar Optuna son las siguientes :


- Integración fácil y escalable.
- Búsqueda dinámica con varios algoritmos eficientes.
- Métodos para obtener visuales de forma rápida de la historia de cada uno de los ensayos, el comportamiento de los hiperparámetros y la importancia de las variables por su ganancia a la respuesta.


Dicho esto, vayamos a explorar el repositorio que construimos para trabajar.


### El repositorio


Todos el código que revisaremos esta hecho en Python y lo encontrarás en el siguiente repositorio para libre uso:


[GitHub - RoSaav/optuna_ml: Proyect for optuna implamentation with XGBoost, lightGBM and CatBoost](https://github.com/RoSaav/optuna_ml/)


Puedes replicar los resultados del repositorio creando un ambiente de python con las especificaciones anexadas en el archivo ‘requirements.txt’, así como instalando las paqueterías necesarias.


El repositorio esta construido de la siguiente manera:


- **data▹breast_cancer.csv**
El conjunto de datos con el cuál trabajaremos para realizar los ejercicios de machine learning se llama ‘Breast Cancer Wisconsin’ donde las variables se calculan a partir de imágenes digitalizadas de estudios en ‘breast mass‘.
Consiste de 569 registros, 32 atributos y el target es el diagnostico {‘Maligno’:1, ‘Benigno’:0}
Si deseas explorar su origen puedes consultarlo en :
https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+%28Diagnostic%29
- **img ▹*.png** Carpeta que contiene el conjunto de imágenes con los resultados de cada iteración.
- **train▹models ▹*.joblib.dat** Carpeta con el conjunto de archivos *.joblib.dat con los modelos guardados en cada iteración.
- **train▹toy.py** Es un programa que tiene como objetivo ayudarnos a entender de una manera simple el funcionamiento de Optuna. Contiene la definición de una función objetivo univariada y otra función objetivo bivariada. **En la siguiente sección profundizaremos el programa** .
- **train▹param_grid.json** Archivo que contiene la definición del campo de hiperparámetros que vamos a utilizar para explorar y optimizar en cada uno de los modelos con Optuna.
- **train▹utils.py** Archivo que contiene principalmente **las funciones objetivo** que utilizaremos para optimizar la búsqueda de hiperparámetros con los modelos.
- **train▹train.py** Es un programa que define la ruta estándar de entrenamiento de un modelo de machine learning. Split, preprocesamiento, y *entrenamiento* . En la sección de *entrenamiento* utilizamos funciones objetivo para resolver el problema de encontrar la mejor hiperparametrización que maximicen la métrica ‘AUC’ en los modelos de XGBoost, LightGBM y CatBoost. **En la última sección profundizaremos el programa** .


Algunos comandos que te podrían ayudar a replicar el código son:


Para crear el ambiente con conda


```text
conda create -n optuna_ml python=3.8.5
```


Para activar el ambiente


```text
conda activate optuna_ml
```


Para instalar los requerimientos (Cerciora que estés en el ambiente y la versión de python que instalaste)


```text
pip install -r requirements.txt
```


Para ejecutar toy.py


```text
python train\toy.py
```


Para ejecutar train.py


```text
python train\train.py
```


### Ejemplo toy.py


En esta sección exploramos el archivo toy.py para entender cada uno de los piezas clave que lo componen, Study, Trial, Direction, Storage y Sampler.


Como ya lo describimos antes toy.py se compone de dos funciones objetivo, bivariada (objective_bi) y univariada (objective_uni), dónde en ambos casos, buscaremos tanto maximizar como minimizar las funciones:


Algunos de los componentes principales son:


- **Study: Una sesión de optimización.** Para una tarea de optimización este componente se encarga de administrar la información sobre qué algoritmo a usar (Sampler), dónde almacenar los resultados del ensayo (Storage) y la dirección de optimización (Minimizar o maximizar) .


```text
#En el ejemplo toy.py línea 18  #study -> Objeto de almacenamiento  #sampler -> BaseSampler (Default)  #direction -> ['maximize', 'minimize']  #optuna.create_study -> Inicialización de estudio
```


```text
study = optuna.create_study(direction=direction)
```


- **Trial: Componente que corresponde a cada ensayo** En la función objetivo define la muestra de parámetros de Optuna proporcionada por el objeto de ensayo y reporta el resultado para realizar la poda.
- **Direction: Componente para definir la dirección de poda** En el study se define para entender la dirección de la poda basado en la función objetivo y los resultados del ensayo.
- **Sampler: Componente para implementar un algoritmo para seleccionar el siguiente parámetro a evaluar** Componente implementa un algoritmo automáticamente para encontrar valores óptimos de los hiperparámetros haciendo uso de distintos tipos de muestreos.
- **n_trials: Componente del número de pruebas a realizar** Una vez definido el objetivo y después de haber inicializado el estudio accionamos la búsqueda definiendo el número de ensayos a través de un número entero


```text
#En el ejemplo toy.py línea 21  #study.optimize -> Método para comenzar la optimización  #objective_bi -> Función objetivo a optimizar  #n_trials -> Número de muestreos o pruebas
```


```text
study.optimize(objective_bi, n_trials=n_trials)
```


- **Storage: Componente que almacena los resultados de las pruebas de optimización.**


Entendimos algunos de los componentes esenciales para el uso de Optuna, ahora, ¿Cómo definimos lo más importante? **, el espacio de búsqueda.**
En nuestro ejemplo juguete, definimos la función objetivo univariada como sigue


```text
def objective_uni(trial):           x = trial.suggest_float('x', -100, 100)           return ( x - 10) ** 2
```


‘x’ es el único elemento sobre el que se estará buscando optimizar para la función (x-10)**2, el ensayo se realizará sobre el espacio \[-100, 100\], ¿en qué punto alcanzamos el mínimo dentro de nuestro espacio de búsqueda?


```text
study = optuna.create_study(direction='minimize')  study.optimize(objective_uni, n_trials=1000)
```


Después de inicializar nuestro estudio y buscar durante 1000 ensayos, encontramos los siguientes resultados.


**Resultado toy.py: Mejores valores de ‘x’ para minimizar después del pruning en cada trial. Best param x =10**


Para nuestra función bivariada ‘x’ y ‘y’ serán los elementos sobre los que haremos la exploración y lo definimos como sigue:


```text
def objective_bi(trial):           x = trial.suggest_float('x', -100, 100)           y = trial.suggest_float('y', -50, 50)           return ( x + y - 10) ** 2
```


El muestreo x lo realizara sobre el espacio \[-100, 100\] y para ‘y’ sobre \[-50, 50\], ¿en que puntos de forma conjunta se maximizan la función ( x + y — 10) ** 2?


**Resultado toy.py: Mejores valores de ‘x’ y ‘y’ para maximizar después del pruning en cada trial.**


Si tuviéramos la necesidad de definir hiperparámetros enteros o categóricos, etc., tendríamos que definir los atributos como sigue.


```text
suggest_categorical  ( name  ,  choices  )  suggest_float  ( name  ,  low  ,  high  ,  *  ,  step=None  ,  log=False  )  suggest_int  ( name  ,  low  ,  high  ,  step=1  ,  log=False  )  suggest_loguniform  ( name  ,  low  ,  high  )  suggest_uniform  ( name  ,  low  ,  high  )
```


Una vez que tenemos claro cada componente del ejemplo juguete continuemos con el ejemplo del modelo.


Para conocer más información sobre las visualizaciones puedes consultar:


[optuna.visualization - Optuna 2.10.1 documentation](https://optuna.readthedocs.io/en/stable/reference/visualization/index.html?highlight=visual#module-optuna.visualization)


### Optuna + Machine Learning


El framework de Optuna tiene la capacidad de integrarse con los módulos más relevantes para aprendizaje automático y aprendizaje profundo, lo cuál **nos permite de una manera simple definir la dirección la poda y detención temprana de ensayos poco prometedores** . En este caso, los módulos a explorar son las siguientes:


Módulos de aprendizaje automático


Si desea comenzar explorando los outputs de la función de entrenamiento en el repositorio (train.py) te doy una breve explicación de los parámetros.


La función principal se llama *train_optuna* y se compone de los siguientes parámetros:


```text
train_optuna(        random_state   = 1337,        test_size   = 0.2,        dataset_path   = 'data/breast_cancer.csv',        metric_name   = 'target',        classifier   = 'lgb',        n_trials   = 50  )
```


- **random_state** : Semilla para asegurar que las acciones en el ambiente sean reproducibles
- **test_size** : Tamaño del entrenamiento
- **dataset_path** : Ruta del set de entrenamiento
- **metric_name** : Nombre de variable de respuesta
- **classifier** : Modelo de clasificación
{‘LightGBM’ :’lgb’, ‘XGBoost’:’xgb’, ‘CatBoost’:’cat’}
- **n_trials** : Número de ensayos


Después de una serie de ensayos podrás encontrar insights de aprendizaje de los modelos en la carpeta de imágenes.


Historia del valor objetivo (AUC) por cada ensayo


### XGBoost


(eXtreme Gradient Boosting) Es una paquetería de software de código libre que provee un framework de refuerzo para gradient boosting.
Mas info en :[https://xgboost.ai/](https://xgboost.ai/)


El primer paso que haremos será explorar la función objetivo para Optuna + Validación cruzada con StratifiedKFold de Sklearn.


La función objetivo contiene los siguientes parámetros


- **trial** : Componente de prueba
- **X** : Set de entrenamiento de variables predictoras
- **y** : Set de entrenamiento de variable de respuesta
- **random_state** : Semilla para asegurar que las acciones sean reproducibles


Aunque parezca un bloque de código pesado lo podemos resumir en dos puntos claves que nos harán entender de forma general como es que Optuna optimiza:


1. El campo de búsqueda


El archivo “train/param_grid.json” filtrado para ‘xgb’ contiene un espacio de los posibles hiperparámetros que son invocados a través de un diccionario directamente en el param_grid. Algunos ellos son por ejemplo, el tipo de “ **booster** ”, \[“gbtree”, “gblinear”, “dart”\] en *trial.suggest_categorical()* o el número de árboles en paralelo ‘ **num_parallel_tree** ’, \[ low=n, high=m, step=1\] en *trial.suggest_int()* . Recordemos que en cada trial solo tomará una muestra de todo el espacio de hiperparámetros.


2. Validación cruzada y entrenamiento


El primer paso consiste en inicializar mediante ‘StratifiedKFold’ el objeto que nos permitirá hacer validación cruzada con nuestro set de entrenamiento mientras aprendemos en nuestros ensayos. El segundo consiste en inicializar ‘XGBoostPruningCallback’ que nos permitirá siempre aprender hacia la mejor dirección de nuestra ‘observation_key’, para nuestro ejemplo ‘validation-auc’, esto es el ‘AUC’ del set de validación en el split previamente realizado.


Por último, como en el ejemplo juguete, tenemos que inicializar el estudio , una vez finalizado, podremos extraer los mejores hiperparámetros en un diccionario con el método ‘study.best_params’.


### LightGBM


( Light Gradient Boosting Machine) Es una paquetería de software de código libre que provee un framework para refuerzo para gradient boosting basado en árboles de decisión para incrementar la eficiencia y el uso de memoria originalmente desarrollado por Microsoft.
Mas info en :[https://lightgbm.readthedocs.io/en/v3.3.2/](https://lightgbm.readthedocs.io/en/v3.3.2/)


De igual forma y sin caer en repetición el primer paso que haremos será explorar la función objetivo.


Cómo se puede observar, la estructura es muy similar sin embargo, la flexibilidad de los módulos nos permiten utilizar los wrappers de sklearn ‘LGMClassifier’.


Al igual que en XGBoost, Optuna cuenta con la intergración de un pruner ‘LightGBMPruningCallback’ que nos permitirá siempre aprender hacia la mejor dirección a partir de la métrica que establezcamos. Por último solo queda inicializar el estudio de forma análoga.


### Notas finales


Aprendimos las principales características de Optuna, así como a definir cada uno de los componentes que nos ayudarán a utilizar el framework para realizar proyectos más rentables en términos de optimización y eficiencia.


Para todos aquellos que sientan interés y quieran profundizar en la documentación, aquí dejo más información.


- [Optuna: A hyperparameter optimization framework - Optuna 2.10.1 documentation](https://optuna.readthedocs.io/en/stable/index.html)
- [GitHub - optuna/optuna: A hyperparameter optimization framework](https://github.com/optuna/optuna)


Algunas fuentes de referencia:


- [Hyperparameter Tuning using Optuna - Analytics Vidhya](https://www.analyticsvidhya.com/blog/2020/11/hyperparameter-tuning-using-optuna/)
- [OPTUNA: A Flexible, Efficient and Scalable Hyperparameter Optimization Framework](https://towardsdatascience.com/optuna-a-flexible-efficient-and-scalable-hyperparameter-optimization-framework-d26bc7a23fff)


Gracias por llegar hasta acá 🤍, ¡nos vemos en la siguiente!


---


[Búsqueda eficiente de hiperparámetros con Optuna + Sklearn para XGBoost y LightGBM](https://engineering.rappi.com/b%C3%BAsqueda-eficiente-de-hiperpar%C3%A1metros-con-optuna-sklearn-para-xgboost-y-lightgbm-96805f1a48ed) was originally published in[Rappi Tech](https://engineering.rappi.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
