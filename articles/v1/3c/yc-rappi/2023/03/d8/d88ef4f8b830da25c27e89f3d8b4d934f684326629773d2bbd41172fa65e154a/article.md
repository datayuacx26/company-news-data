---
schema_version: "1.0.0"
document_id: "d88ef4f8b830da25c27e89f3d8b4d934f684326629773d2bbd41172fa65e154a"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/planificaci%C3%B3n-de-tareas-scheduler-4250961fa944"
published_at: "2023-03-21T15:42:51+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-08-20T02:17:23.302365+00:00"
content_hash: "sha256:1382184d0d38f77f69046faf6d48e6c95932865936331647e91f2e8b01443162"
---

# Planificación de tareas (Scheduler)

### Introducción


En el ámbito de los sistemas de software existe una necesidad recurrente que involucra planificadores que permitan ejecutar tareas en algún punto del futuro cercano. Estas no están limitadas a una familia de tareas similares sinó que pueden abarcar prácticamente cualquier tipo de acción dentro de la plataforma en la que se ejecutan.


En una plataforma dirigida por eventos estos planificadores permiten ahorrar recursos computacionales que de otra forma deberían malgastarse escuchando los eventos que arriban por algún tipo de canal y terminar filtrando solo unos pocos de ellos, que son los de interés. Como alternativa, en lugar de procesar todos los eventos a medida que arriban, reaccionamos a la expiración de un temporizador relacionado a la tarea de, por ejemplo, validar que algo haya sucedido ya. Un ejemplo claro puede ser el de efectuar una validación de cambio de estado 5 minutos después de la transición a un estado intermedio. En lugar de procesar todos los eventos, creamos un temporizador y a los 5 minutos consultamos al sistema si el estado anterior ha cambiado al nuevo estado esperado.


En este documento hablaremos sobre el servicio llamado **delayed-tasks** que es el planificador multipropósito de Rappi, la solución técnica a la que arribamos y los conceptos aplicados durante su desarrollo.


### ¿Qué es un planificador?


Cuando hablamos de un planificador, hablamos de un componente que nos permite asociar temporizadores a tareas y que reacciona a la expiración de estos temporizadores ejecutando su tarea asociada. Debe cumplir con dos requisitos fundamentales:


**▶︎ Ser preciso** : De nada sirve un planificador que reacciona a la expiración de un temporizador a destiempo. Aunque podemos darnos cierta licencia de algunas pocas “unidades de tiempo” de demora ya que está claro que la resolución con la cual opera el planificador validando la expiración de los temporizadores puede introducir una pequeña desviación si la creación de estos temporizadores permite definir su expiración con una precisión muy alta.


**▶︎ Ejecutar tareas en paralelo** : Cuando cada una de las tareas a ejecutar define su propio temporizador un número arbitrario de ellas puede expirar en idéntica fracción de tiempo. Sin paralelismo terminaríamos ejecutando las tareas de forma serializada y penalizando la ejecución de cada tarea con la latencia de la tarea previa, lo que reduce drásticamente la utilidad del planificador en plataformas con alta demanda.


Luego podemos agregar las características adicionales que nuestro planificador deba cumplir, pero estas dos son fundamentales en todos los casos.


Por otro lado debemos definir el tipo de tareas que ejecutará el planificador una vez que el temporizador asociado a ella expire:


**▶︎ Invocaciones HTTP** : En la primera etapa, las tareas ejecutadas por el planificador serán llamados HTTP. La definición de la tarea contemplará todos los atributos necesarios para configurar dichas invocaciones.


**▶︎ De corta duración** : Al ejecutarse la tarea esta debe garantizar un tiempo de respuesta de milisegundos. Para tareas de larga duración (más allá de los X segundos a definir) contamos con otros mecanismos, como por ejemplo Cron, Control-M, Task scheduler.


Existen diferentes algoritmos que podemos utilizar para implementar un planificador, algunos más simples, otros más complejos, pero en esencia podemos reducir las distintas soluciones al siguiente gráfico, aunque en lo implementativo pueden variar.


Vista conceptual


Como vemos en el gráfico precedente, a medida que el tiempo pasa los temporizadores expiran, se eliminan de la lista y se ejecuta la tarea asociada a cada uno de ellos.


Algunos planificadores pueden utilizar listas ordenadas para detectar la expiración de los temporizadores, otros pueden utilizar estructuras del tipo anillo, incluso de[anillos concéntricos](https://www.confluent.io/blog/apache-kafka-purgatory-hierarchical-timing-wheels/) , pero en esencia es tan simple como detectar si durante el tiempo transcurrido entre dos ticks consecutivos de evaluación del planificador se produjo expiración o no.


Una interesante fuente de información sobre los tipos de algoritmos para planificadores y que de hecho fue una de nuestras referencias es[este artículo](https://blog.acolyer.org/2015/11/23/hashed-and-hierarchical-timing-wheels/) , basado en[este documento](http://www.cs.columbia.edu/~nahum/w6998/papers/sosp87-timing-wheels.pdf) .


### Requisitos principales


Todas las organizaciones tienen una serie de requisitos a cumplir para considerar una solución oportuna. En nuestro caso, la lista de requerimientos para el nuevo planificador era extensa, pero los más importantes son:


1- Debe soportar tareas de una única ejecución así como tareas recurrentes, es decir, una vez expirado el temporizador inicial debe volver a ejecutarse de forma periódica la misma tarea.


2- La ejecución de las tareas debe ser auditable y generar un histórico de ejecuciones con el resultado de las mismas.


3- La desviación entre la expiración del temporizador y la ejecución de la tarea asociada debe ser, como máximo, 1 segundo


4- Las tareas serán llamados HTTP, permitiendo definir el verbo HTTP, payload, URL y headers, así como una expiración de la petición.


5- Debe exponer un API REST para permitir la creación, consulta y cancelación las tareas. Este API debe ser escalable para poder ajustarse de forma dinámica a la carga del sistema.


6- El stack tecnológico debe limitarse al usado por la compañía, de forma que no introduzca complejidad adicional en las fases de mantenimiento y despliegue en la plataforma.


7- Debe diferenciar entre tareas de alta y baja prioridad y garantizar que las tareas de alta prioridad serán ejecutadas incluso cuando el planificador tenga un fallo parcial en alguno de sus componentes y aún en desmedro de las tareas de baja prioridad.


8- El planificador debe asegurar la semántica “exactly once” en la ejecución de tareas, por lo que está prohibido procesar el mismo temporizador más de una vez.


### Diseño de alto nivel


Diseñar una solución no es más ni menos que asignar responsabilidades a los componentes de un sistema. Entonces, lo primero que debemos hacer es entender cuáles son esas responsabilidades, como se agrupan o desacoplan y cual es la mejor estrategia para facilitar el desarrollo y mantenimiento del sistema en construcción.


En este aspecto y viendo las responsabilidades del planificador desde el nivel de abstracción superior solo son 2:


**▶︎ Administrar tareas** : Mediante un API REST permitiremos crear tareas, consultar el histórico de ejecuciones, pendientes y cancelarlas.


**▶︎ Persistir tareas** : De esta forma no será necesario, aunque en todo caso tampoco es conveniente, mantener las tareas en memoria hasta alcanzar la expiración del temporizador asociado. Se persistirán en un repositorio centralizado y se “consumirán” desde allí.


**▶︎ Ejecutar tareas** : **** Contaremos con un proceso dedicado, encargado de ejecutar las tareas en paralelo para lo que utilizaremos una cantidad de instancias variable.


Como vemos, empieza a surgir la distribución física del planificador que hasta ahora contará con tres componentes, uno del tipo API REST, otro del tipo Worker y el componente de persistencia de las tareas, presumiblemente algún tipo de base de datos.


Funcionamiento esquemático


Está claro que en cualquier sistema que no sea trivial es necesario asegurar la durabilidad de la información mediante persistencia y redundancia. Este planificador no es la excepción. Luego, cuando contemos con más información, decidiremos cómo hacerlo, ya que uno de los principios fundamentales del diseño de sistemas es el de recabar la mayor cantidad de información posible antes de tomar decisiones sobre las tecnologías a utilizar.


Ahora hablaremos del único aspecto interesante del API REST del planificador y luego nos enfocaremos en el componente al que llamamos worker para finalmente analizar las necesidades de persistencia.


### API Rest


Para poder priorizar tareas obviamente necesitamos poder clasificarlas en grupos prioritarios y no prioritarios, de forma que una vez clasificadas estemos en condiciones de asignarles los recursos necesarios.


**¿Pero cómo clasificar las tareas?**


En Rappi no tenemos tráfico de red anónimo entre servicios ya que al comunicarse unos con otros se inyecta por plataforma un identificador de aplicación. Entonces, utilizando este identificador sabemos que parte del sistema es responsable de la creación de cada una de las tareas a planificar y de allí, la criticidad de las mismas.


La información para poder clasificar las tareas formará parte de la configuración del planificador y este será el único responsable de decidir a qué grupo pertenece cada una de las tareas, previniendo situaciones tales como un exagerado número de tareas prioritarias o errores en la definición de la prioridad de las tareas si delegamos esta responsabilidad en los clientes, por ejemplo, como parte de la información recibida en la creación.


Por último, todas aquellas tareas que no sea posible clasificar, serán asignadas a una la clasificación por defecto, lo que nos permite configurar solo las tareas que nos interese separar del resto y entre sí.


En resumen, la selección automática de prioridad por clasificación funcionará de la siguiente manera


Ruteo por prioridad


### Worker


Con diferencia, las principales características que debe implementar el worker del planificador son aquellas relacionadas a la performance y resiliencia.


Por el lado de la performance, un buen inicio es el de poder agregar y quitar instancias para reaccionar a los cambios de carga de trabajo que indefectiblemente sucederán. Pero esto por sí solo no es suficiente ya que de no implementar ejecución concurrente necesitaremos un gran número de instancias para que cada una ejecute tareas en serie.


Distribución de carga y concurrencia


Una vez que el componente API Rest del planificador durante la creación de la tarea ha asignado el grupo correspondiente, cada tarea incluirá esta información de forma que el worker pueda accederla para identificar la prioridad correcta y ejecutar en el espacio de recursos correspondiente.


### Mecanismo de expiración de los temporizadores


Como dijimos anteriormente, el planificador funcionará asociando temporizadores con tareas. Es necesario seleccionar el mecanismo mediante el cual podamos verificar cuales son los temporizadores expirados para ejecutar su tarea asociada y esto debe ser en tiempo real.


Existen diferentes tipos de estructuras y algoritmos en los que podemos apoyarnos para cumplir con esta mecánica, pero por cuestiones de complejidad en el desarrollo y mantenimiento, así como otras limitaciones, no todos son aplicables en la plataforma de Rappi.


Por ejemplo, los **Temporizadores en memoria** utilizan estructuras de tipo buffer circulares o similares, apoyados además con algún mecanismo de hashing para mejorar el tiempo de acceso a los temporizadores, pero todas las soluciones que se ejecutan en memoria quedan descartadas ya que en Rappi utilizamos instancias del tipo[SPOT de Amazon](https://aws.amazon.com/es/ec2/spot/) , lo que implica que dichas instancias pueden ser reclamadas y reasignadas en cualquier momento, generando la pérdida de los temporizadores mantenidos por la instancia.


Dicho lo anterior, es importante aclarar que las instancias del worker no tendrán ningún tipo de estado ya que pueden ser reclamadas en cualquier momento y no siempre de forma amigable, lo que implica que tampoco podemos utilizar descarga a disco del estado de una instancia.


Resta evaluar la implementación de una **lista ordenada de temporizadores** , cuyo orden viene dado por el valor absoluto de expiración. El planificador en cada iteración obtiene el primer elemento de la lista en el extremo de temporizadores cuyo instante de expiración es el menor y lo compara con el instante actual. Si la expiración del temporizador es inferior al instante actual, se remueve de la lista y se ejecuta la tarea relacionada.


Expiración de temporizadores y ejecución de su tarea asociada


La lista ordenada de temporizadores tiene un costo de inserción de nuevos elementos O(n) ya que debe insertarlo en la posición correcta, pero cuando el valor de “n” es alto, es decir que tenemos muchos elementos en la lista ordenada, podemos usar **árboles de temporizadores** ya que con ellos logramos reducir el costo de la operación de inserción a O(log(n)).


### Persistencia


En cuanto a la persistencia y debido a los requerimientos de desempeño y los posibles algoritmos de planificación, debemos encontrar una base de datos que tenga una latencia extremadamente baja y que nos permita implementar una lista ordenada de temporizadores.


Una restricción adicional que encontramos aquí es el hecho que no es posible usar la expiración como clave principal o índice único ya que diferentes temporizadores pueden expirar en el mismo instante.


La primera opción a evaluar es una base de datos relacional, en el caso de Rappi puede ser bien PostgreSQL o MySQL/MariaDB.


En primer lugar **PostgreSQL** no garantiza orden, de hecho las filas de una tabla pueden, con el tiempo, cambiar de localización, por ejemplo luego de una actualización. Fin de la evaluación.


Luego evaluamos **MySQL** y **MariaDB** . En este caso solo tenemos en cuenta el motor InnoDB y observamos que utiliza índices que definen el ordenamiento físico de los registros en la tabla (clustered index) pero con la limitación de ser aplicable solo a la clave principal de la tabla y en caso de que la tabla no tenga clave principal, es aplicable solo al primer índice único de la misma. Fin de la evaluación.


> En todo caso, los RDBMS no garantizan orden en los registros a menos que se utilice la cláusula **ORDER BY,** que tiene un costo computacional adicional.


Ahora veamos bases de datos no relacionales.


**MongoDB** inserta los nuevos documentos en forma secuencial y contigua a los ya existentes, por lo que no reordena físicamente las colecciones. Adicionalmente, operaciones como FindAndModify son bloqueantes, por lo que son imposibles de usar en situaciones en las que la carga de trabajo aumenta significativamente. Fin de la evaluación.


**DocumentDB** la competencia a MongoDB de Amazon. En base a la experiencia que tenemos en Rappi, es menos performante que MongoDB y no reacciona bien ante los aumentos súbitos de carga. Fin de la evaluación.


Pasamos luego a ****[Redis](https://redis.io/) **,** que si bien se considera un “[data store](https://en.wikipedia.org/wiki/Data_store) ” el término es tan amplio que aplica a todo aquello capaz de guardar datos, por lo que está a la par de un archivo, una base de datos relacional, una documental, etc.


En este análisis consideramos a Redis como una base de datos clave/valor no durable **,** de la cual podemos configurar un esquema de servidor primario con una réplica (secundario) distribuidos en múltiples AZ (recordemos que en Rappi trabajamos con AWS) y aunque funcione en memoria suele ser muy confiable por lo que tenemos pocas preocupaciones al respecto.


No es para sorprenderse lo rápido que se ejecutan las operaciones en Redis debido a que trabaja en memoria. En cuanto al soporte para la lista ordenada de temporizadores, Redis entre las estructuras de datos que implementa nos proporciona una llamada **ZSet** (set ordenado) que es, justamente, la lista ordenada que necesitamos ya que agrega a cada valor del set un score que es un número flotante y cuya función es ordenar el set.


Además, Redis utiliza un modelo de ejecución mono-hilo en el cual cada comando es bloqueante, lo que nos permite pensar en la búsqueda y extracción de un temporizador expirado sin el peligro de sufrir condiciones de carrera al utilizar múltiples instancias del worker.


Nos queda un requisito importante a cumplir que es el de auditoría y es importante ya que vamos a construir un servicio sobre el cual muchos otros servicios delegarán una parte de su procesamiento, por lo que es necesario contar con todas las herramientas que nos permitan realizar un análisis de fallas del planificador o de las tareas que éste ejecuta. Por ello sumaremos una base de datos relacional (MySQL) que nos permitirá sumar redundancia y funcionará como copia de rescate cuando suceda una catástrofe con Redis.


No está de más comentar que el modelado de datos de ambos motores de persistencia será diferente y estará optimizado para cada uno de ellos y que una falla en la base de datos relacional no puede ni debe afectar al planificador y su misión de ejecutar tareas, por lo que preferentemente toda interacción con la base de datos relacional será asíncrona y seguirá su propio flujo de ejecución.


Expuesto todo lo anterior, podemos comenzar a delinear los componentes que compondrán al planificador


Esquema de distribución de componentes


### Tecnologías


Afortunadamente, del análisis de requerimientos ya han surgido las tecnologías de la capa de persistencia por lo que resta definir el lenguaje de desarrollo y los frameworks relacionados.


El equipo responsable por el desarrollo y mantenimiento del planificador trabaja habitualmente con Kotlin, Go y Scala, siendo por mucho Koltin el lenguaje principal.


Entonces, el stack tecnológico queda definido de la siguiente forma:


- Kotlin + Arrow-kt
- Ktor como marco web
- Corrutinas + funciones suspendidas en la implementación de concurrencia
- Kotest como marco de pruebas
- Redis como persistencia primaria
- Jedis para la interacción con Redis
- MySQL como persistencia secundaria y recuperación en caso de desastre
- JDBC + Hikary para la comunicación con MySQL
- Json como formato de intercambio de mensajes entre los clientes y el planificador


Nada de todo esto es innovador. Priorizamos la madurez de cada componente del stack a fin de reducir al mínimo la posibilidad de encontrarnos con sorpresas desagradables, además de reducir la[complejidad accidental](https://es.wikipedia.org/wiki/Accidental_complexity) .


Teniendo en cuenta que la tecnología es el medio, pero no el fin, no resta nada adicional por mencionar, entonces pasamos a explicar los principales mecanismos utilizados en la implementación del planificador


### Fase de Implementación


Sin dudas lo más importante en la fase de implementación es el proceso de comprobación de expiración de los temporizadores y la ejecución de las tareas asociadas a ellos ([ver gráfico](https://cdn-images-1.medium.com/max/1600/1*oyhfcITEz_Bcrnj4UdgQRA.png) ).


#### Lista ordenada de Temporizadores


Usaremos la estructura ZSet de Redis (Set ordenado) en la que la expiración de cada temporizador será el momento exacto en el tiempo para lo que usaremos el[EpochMilli](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#toEpochMilli--) de la clase[Instant](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html) de la JDK. El sistema usará UTC.


Por razones de performance será conveniente usar el ZSet como un índice cuyo valor sea la clave de Redis que contiene toda la información relacionada con la tarea a ejecutar ya que bien es posible usar como valor del ZSet la definición de la tarea, la inserción se verá penalizada por el tamaño del texto ya que debe comprobarse que no exista con anterioridad.


Distribución física en Redis


Adicionalmente, hacerlo de esta manera nos permite definir un TTL para cada clave con el grueso de la información de las tareas previendo que estas claves perduren indefinidamente debido a algún problema durante la ejecución del proceso.


#### Expiración de temporizadores


El modelo mono-hilo de ejecución de Redis nos asegura que las operaciones son ejecutadas en forma secuencial, pero como utilizamos un modelado de datos que debe ejecutar solo una operación cuando ningún temporizador expira pero tres operaciones cuando un temporizador si lo hace, corremos el riesgo de enfrentar una condición de carrera ya que más de un worker es capaz de consumir el mismo temporizador expirado resultando en duplicidad de ejecuciones, como se puede apreciar en el siguiente pseudo-código:


```text
while true    get a redis connection     execute a zrangeByScore on ZSet from -inf to now       pick first result from list //can be empty      if result exists then         remove key from ZSet          execute get using key to get task         send task to asynch execution    end if    wait x millis to continue  end while
```


Las líneas resaltadas son los comandos que enviamos al Redis, de los cuales los dos primeros son los más peligrosos ya que entre el zrangeByScore y el remove puede ejecutarse otro zrangeByScore de una instancia diferente del worker y duplicar la ejecución de la tarea asociada al temporizador expirado.


Para solucionar esto utilizamos el mismo concepto de ejecución serializada de Redis, y que también aplica a la ejecución de[scripts LUA](https://redis.io/docs/manual/programmability/eval-intro/) , utilizando el siguiente script, que ejecuta todos los pasos de pseudo-código previo, pero en una única llamada


```text
local result = redis.call('ZRANGEBYSCORE', KEYS[1], 0, ARGV[1], 'limit', 0, 1)  local member = result[1]  if member then      redis.call('ZREM', KEYS[1], member)      local task = redis.call('GET', member)      return task  else      return ''  end
```


y ejecutando una precarga en el código del worker al momento de inicializar el repositorio Redis


```text
private val scriptSha1 = client.resource.use { jedis ->     jedis.scriptLoad(luaScript)  }
```


Obtenemos un hash con el cual podemos repetir la ejecución del script ya que es la clave de la caché interna de Redis donde dicho script se encuentra cargado y pre-compilado.


Una vez cargado el script, solo debemos pasarle los parámetros correspondientes para obtener una tarea cuyo temporizador haya expirado, todo en una única llamada.


Si no se encuentra un temporizador expirado la ejecución retorna una cadena de caracteres vacía.


```text
val task = client.resource.use { jedis ->      jedis.evalsha(          scriptSha1, //LUA script id          listOf(queueKey), //ZSet name          listOf(score.toString()) //now()      )  }
```


### Prioridad de tareas


Como dijimos con anterioridad, todos los clientes del planificador son identificables mediante su ID de aplicación, el cual es inyectado por la plataforma.


Con esta información, fácilmente podemos clasificar las tareas y definir, mediante configuración, como son tratadas las tareas del cliente A en contraposición con las tareas del cliente B y decir, por ejemplo que las tareas de A son menos prioritarias que las tareas de B.


Las tareas agrupadas por prioridad contarán con recursos del sistema asignada de forma exclusiva (a los que llamamos SPACE) y de esta forma aplicaremos el patrón[bulkhead](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead) **,** para que un group de tareas no sea capaz de afectar a los otros cuando los problemas aparezcan y todos sabemos que, en un entorno cloud, tarde o temprano los problemas aparecen en sus más variadas formas.


Cada SPACE cuenta con su Redis dedicado y el repositorio Redis del worker utiliza el SPACE para discernir el pool de conexiones a utilizar. Este conjunto de pools de conexiones se mantiene en un mapa SPACE -> Redis pool.


Es importante mencionar que todo aquello que no se haya configurado para utilizar un SPACE en particular termina utilizando un SPACE por defecto.


Ruteo por prioridad en la creación de la tarea


Adicionalmente, como las tareas se limitan a la invocación de endpoints de la plataforma, cada instancia del worker utiliza diferentes clientes HTTP configurados según la prioridad de las tareas que ejecutan. Estos clientes HTTP se identifican por su prioridad (LOW, MEDIUM, HIGH) y se diferencian, por ejemplo en los timeouts, cantidad de conexiones máxima, threadpool executor asignado y un no muy extenso etc…


A medida que aumenta la carga de trabajo se apagan en primera instancia todos los procesos relacionados a la tareas de baja prioridad, incluidos el cliente HTTP asignado y su threadpool correspondiente, a la vez que se envía una señal de alerta a la plataforma de monitorización y observabilidad que utilizamos en Rappi.


Lo mismo sucede con las tareas de prioridad MEDIUM si la carga de trabajo continúa en aumento.


Una vez que la carga comienza a normalizarse, se vuelven a encender los procesos asignados a las tareas MEDIUM y LOW en ese orden. Vale destacar que, la tareas deben indicar un período más allá del cual la misma se considera expirada y no se ejecuta, de forma que al normalizarse la operación puedan descartarse aquellas que no sea estrictamente necesario ejecutar. Todas aquellas tareas que no definan una expiración, al momento de encenderse los flujos de baja prioridad serán ejecutadas.


Este mecanismo se apoyará en un circuit breaker con una función de testeo de carga.


### Tareas recurrentes


Este tipo de tareas una vez iniciadas se ejecutan periódicamente. Suelen ser de baja frecuencia y no muy numerosas en una plataforma event-driven, pero existen y el planificador debe darles soporte.


Definir la frecuencia de las mismas es bastante sencillo, solo es necesario indicar el instante en el cual debe ejecutarse la primera de ellas, las repeticiones se especifican mediante una duración, por ejemplo 2 minutos, y opcionalmente un instante en el cual debe detenerse.


Una de las responsabilidades del planificador es el de asegurar que una tarea recurrente no se detiene a menos que así se haya configurado en la misma o se cancele vía el API de administración.


La primera opción que exploramos durante la implementación fue la de tratar estas tareas de igual manera que cualquier otra, pero lanzando una corutina adicional que, en paralelo a la ejecución de la tarea crease la siguiente repetición de ser necesario. Esta corutina se lanza antes de efectuar cualquier otra actividad en la ejecución de la tarea, ya que las repeticiones no dependen del resultado de la ejecución de la tarea actualmente en proceso.


Si bien ejecutar el cálculo y creación de las repeticiones de forma concurrente tiene el doble propósito de no retrasar la tarea en ejecución y crear la nueva tarea lo antes posible, aún así existe la posibilidad de cortar la cadena de repeticiones si nos encontramos con una situación que detenga el procesamiento, como un reclamo de instancias por parte de Amazon o un micro-corte con el mecanismo de persistencia.


Para mitigar este problema es que durante la creación de tareas y si la tarea tiene repeticiones, como estrategia para aumentar la resiliencia en general creamos la tarea inicial y un número de las tareas subsiguientes, de esta manera, si alguna de ellas falla, quedan tareas futuras pendientes de ejecución y no se detiene la cadena de repeticiones. Adicionalmente, el proceso concurrente de creación de repeticiones, en lugar de crear la siguiente tarea, crea la N+1 de forma que siempre tendremos N+1 tareas posteriores. Podemos pensarlo como si se tratase de un buffer de repeticiones que nos proporciona tolerancia a los fallos.


Las tareas recurrentes crean más de una en el futuro


### Recuperación ante desastres


Quienes vivimos la época del “on premise” que en latinoamérica duró hasta bien entrada la década del 2010, sabíamos que por su naturaleza Redis era una caché volátil.


Nada que necesitase una persistencia duradera debía guardarse en Redis ya que bastaba con que la señora de limpieza sin intención desconectara los cables equivocados para cortara la alimentación de, justamente, el servidor donde se ejecutaba nuestro Redis y perdíamos toda la información en él. Esto que menciono como ejemplo lo he vivido en carne propia durante una guardia nocturna hace años.


De allí que todo aquello que requiriese una persistencia no volátil siempre lo construyeramos pensando en BBDD relacionales o, incluso, sistemas de archivos, todo con backup (por lo general en cinta) y Redis quedaba limitado a una capa de caché de duración limitada y que no fuese costoso reconstruir desde cero.


Esta introducción nos permite entender cómo han cambiado los entornos de ejecución con los años y como, a su vez, han cambiado las herramientas en estos nuevos entornos.


Por ejemplo, AWS hasta el 13 de enero de este año (2023) aseguraba por contrato una disponibilidad de ElastiCache para Redis con multi-az del 99.9% y a partir de ese día se asegura una disponibilidad diez veces superior, es decir 99.99%.


¿Queremos decir con esto que no hace falta nada más que Redis?


Claramente **NO** . Siempre debemos diseñar los componentes críticos de forma redundante, y protegernos de los fallos que pueden producirse en estos componentes o eventuales problemas de nuestro proveedor de servicios en la nube.


Teniendo todo esto en cuenta es que como mencionamos con anterioridad, el planificador contará con redundancia doble. Por un lado usando Redis replicado y por el otro sumando una BBDD relacional, también replicada.


**La recuperación ante desastres funcionará de una forma bastante sencilla** :


1- Cuando el sistema esté funcionando correctamente, todas las operaciones sobre temporizadores se ejecutarán tanto en Redis como en MySQL, pero en este último de forma asíncrona para no agregar los tiempos de respuesta de la BBDD relacional a los tiempos de respuesta de Redis. Esta ejecución asíncrona se basará en corrutinas de Kotlin y funciones suspendidas.


Espacios de ejecución múltiples


2- Ante una pérdida de servicio de MySQL, el planificador funcionará en modo de fallo parcial, suspendiendo las operaciones sobre este y sólo quedará activo Redis. Durante este lapso de tiempo, corremos el riesgo de pérdida de servicio de Redis pero mitigado por la configuración primario/secundario multi-az. Aún así será de suma importancia recuperar el servicio de MySQL. Un riesgo que para la primera versión del planificador estamos dispuestos a asumir.


Fallo de MySql en el espacio A


3- Si ocurre un fallo con Redis, pero solo de un nodo primario, por failover comenzaremos a utilizar alguno de los nodos secundarios en otra AZ.


Fallo de Redis en el espacio A


4- Existe un escenario adicional en el cual necesitamos migrar algún SPACE en particular a un nuevo Redis, ya sea por problemas de dimensionamiento o de priorización de alguna aplicación que requiera asignar recursos exclusivos cuando no los tenía. En este caso recuperaremos la información para construir el nuevo Redis desde MySQL. Para esto es que tenemos la información duplicada y actualizada, por lo que bastará cambiar la configuración provista por el servicio de configuración remota en tiempo real del planificador con los nuevos nodos Redis, las instancias API Rest y worker re-apuntarán a la nueva AZ y lanzaremos un proceso que de forma secuencial recorrerá la tabla de temporizadores en MySQL y los persistirá en Redis.


Migración de un conjunto de tareas a un nuevo espacio


De esta manera, ante un fallo catastrófico o la necesidad de re-alocación, la pérdida de servicio del planificador será reducida en el tiempo y una vez recuperado continuará con las operaciones desde el punto dónde el evento se produjo.


### Resumen


Hemos compartido hasta aquí lo que consideramos de mayor importancia a la hora de implementar un planificador de tareas capaz de soportar decenas de miles de eventos diarios, con alta disponibilidad, tiempos de ejecución realmente reducidos y capaz de administrar la carga de forma inteligente.


Nos han quedado cosas afuera, pero es imposible en este formato de documento describir de forma pormenorizada un planificador como el de Rappi.


Esperamos que nuestra experiencia sirva de guía a quienes se encuentren en la tarea de desarrollar un mecanismo similar y que lo compartido aquí sume conocimientos tanto en ideas de diseño como en planificación de redundancia y recuperación.


Invitamos al lector a que ante la necesidad de implementar un mecanismo similar pueda mejorar este diseño y lo comparta con la comunidad a fin de sumar conocimiento colectivo.


---


[Planificación de tareas (Scheduler)](https://engineering.rappi.com/planificaci%C3%B3n-de-tareas-scheduler-4250961fa944) was originally published in[Rappi Tech](https://engineering.rappi.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
