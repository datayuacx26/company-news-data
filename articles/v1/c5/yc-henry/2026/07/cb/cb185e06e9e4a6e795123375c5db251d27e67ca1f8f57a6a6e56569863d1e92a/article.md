---
schema_version: "1.0.0"
document_id: "cb185e06e9e4a6e795123375c5db251d27e67ca1f8f57a6a6e56569863d1e92a"
company_key: "yc-henry"
company: "HENRY"
source_id: "yc-henry-rss-d34ee96ffff8"
canonical_url: "https://blog.soyhenry.com/sql-para-data-scientists-las-10-consultas-que-mas-te-van-a-pedir-en-una-entrevista/"
published_at: "2026-07-22T12:43:13+00:00"
first_seen_at: "2026-07-25T07:55:58.830099+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:0967a0fcaab13df9449d972926fcdc088494e963cf487b5148d85b8f8f0556f5"
---

# SQL para Data Scientists: las 10 consultas que más te van a pedir en una entrevista

En casi toda entrevista para un puesto de datos, SQL es la primera puerta. No importa si el rol es de análisis, ciencia de datos o machine learning: en algún momento —casi siempre en la ronda técnica— aparece un ejercicio de SQL, y muchos candidatos fuertes en teoría tropiezan justo ahí.


En esta nota te contamos las 10 consultas que más te van a pedir, con la técnica clave de cada una, para que llegues a la entrevista reconociéndolas de entrada.


## **Por qué SQL es el filtro de casi toda entrevista de datos**


En una entrevista de datos, SQL no mide si memorizaste la sintaxis, sino si **puedes traducir una pregunta de negocio en una consulta correcta y eficiente** . Por eso los ejercicios rara vez piden "explica ROW_NUMBER": piden "encuentra el mejor pagado de cada departamento", y esperan que reconozcas solo el patrón que hay detrás.


**Esto cambia la forma de prepararse.** No se trata de acumular consultas de memoria, sino de entrenar el reconocimiento de patrones: cuando escuchas "el segundo más alto", "el mejor de cada grupo" o "comparado con el mes anterior", tu cabeza tiene que saltar directo a la técnica correcta.


Aquí te contamos las 10 preguntas que hacen hoy en el mercado laboral y te conviene dominar.


## **Cómo es la instancia técnica donde vas a resolver esto**


SQL casi nunca es toda la entrevista: es parte de la ronda técnica, dentro de un proceso que suele tener varias etapas (un filtro inicial, una o más rondas técnicas de SQL o Python, preguntas de estadística o machine learning y, a veces, un caso de negocio).


Esa ronda técnica toma, por lo general, uno de tres formatos:


- **Live coding:** compartes pantalla y escribes las consultas en vivo mientras explicas tu razonamiento. Es el formato más común, y pensar en voz alta cuenta tanto como el resultado.
- **Prueba online:** resuelves ejercicios en una plataforma tipo HackerRank o CoderPad, muchas veces con tiempo límite.
- **Verbal o en pizarra:** describes cómo escribirías la consulta sin llegar a ejecutarla.


Saber en qué formato vas a estar cambia cómo te preparas: para live coding, practica explicando en voz alta; para una prueba online, cronométrate mientras resuelves.


*Si quieres aprender SQL aplicado a problemas reales de datos, y no en ejercicios sueltos, la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=sqlDataScientistEntrevistaConsultas) *te forma con proyectos que te preparan para este tipo de entrevistas.* 🚀


## **Los cinco fundamentos que aparecen en casi toda entrevista**


#### **1️⃣ JOINs: INNER vs LEFT (y encontrar lo que falta)**


Lo primero que prueban. Un INNER JOIN devuelve solo las filas que coinciden en ambas tablas; un LEFT JOIN conserva todas las de la izquierda aunque no haya coincidencia. La pregunta trampa clásica es "encuentra los clientes que nunca compraron", que se resuelve con un **LEFT JOIN pedidos ON ... WHERE pedidos.id IS NULL** . Su variante, el self-join, también aparece seguido: "empleados que ganan más que su jefe", uniendo la tabla consigo misma.


#### **2️⃣ Agregar y filtrar grupos: GROUP BY + HAVING**


GROUP BY colapsa las filas en grupos para aplicar COUNT, SUM o AVG; HAVING filtra esos grupos ya agregados, a diferencia de WHERE, que filtra antes de agrupar. El ejemplo típico es "clientes con más de 3 compras": **GROUP BY cliente_id HAVING COUNT(*) > 3** . El error más común, y que el entrevistador espera ver si cometes, es intentar filtrar el agregado con WHERE.


#### **3️⃣ El segundo salario más alto (el clásico de los clásicos)**


Es, literalmente, la pregunta de SQL más repetida en entrevistas. Te piden el valor que sigue al máximo, cuidando los empates y el caso "no existe". La forma robusta y portable entre bases de datos es una subconsulta: **SELECT MAX(salario) FROM empleados WHERE salario < (SELECT MAX(salario) FROM empleados)** . También sirve ordenar de mayor a menor y saltar el primero (ORDER BY salario DESC LIMIT 1 OFFSET 1), o usar DENSE_RANK, y poder explicar la diferencia entre esos caminos suma puntos.


#### **4️⃣ Rankear por grupo: ROW_NUMBER, RANK y DENSE_RANK**


Las funciones de ventana ya no se consideran "avanzadas": se dan por sabidas. La pregunta estrella es "el mejor pagado de cada departamento": **ROW_NUMBER() OVER (PARTITION BY departamento ORDER BY salario DESC)** y filtrar donde el número de fila es 1. La trampa está en los empates: RANK deja huecos después de un empate, DENSE_RANK no, y ROW_NUMBER nunca empata. Elegir la función equivocada es uno de los errores que más descartan candidatos.


#### **5️⃣ GROUP BY vs funciones de ventana: colapsar o conservar filas**


Quizá la distinción más valorada hoy. GROUP BY devuelve una fila por grupo y pierde el detalle individual; una función de ventana calcula el mismo agregado pero conserva todas las filas. Si te piden "cada empleado junto al promedio de su departamento y su diferencia contra ese promedio", con GROUP BY solo no se puede: necesitas **AVG(salario) OVER (PARTITION BY departamento)** . Explicar este contraste demuestra que entiendes cómo funciona el motor, no solo la sintaxis.


*Dominar estos fundamentos con proyectos reales, y no con ejercicios aislados, es exactamente lo que hace la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=sqlDataScientistEntrevistaConsultas) *, con mentores que trabajan en la industria.* 💡


## **Las cinco que demuestran dominio real**


#### **1️⃣ Agregación condicional con CASE WHEN**


Es la navaja suiza del análisis y, en Data Science, la herramienta para construir variables. CASE WHEN permite contar o sumar por categoría dentro de una sola consulta: **SUM(CASE WHEN estado = 'activo' THEN 1 ELSE 0 END)** cuenta los activos, y varios de estos en un mismo SELECT arman una tabla pivote. Aparece en preguntas de segmentación y de creación de features para un modelo.


#### **2️⃣ Series temporales: acumulados y comparar con el período anterior**


El análisis de datos vive de comparar en el tiempo, y dos patrones se piden mucho. El acumulado se resuelve con **SUM(ventas) OVER (ORDER BY fecha)** ; la comparación contra el período anterior, con **LAG(ventas) OVER (ORDER BY mes)** , que permite calcular, por ejemplo, el crecimiento mes a mes. La versión difícil es la de "rachas" (días consecutivos de actividad), que combina LAG con sumas acumuladas.


#### **3️⃣ Encontrar y eliminar duplicados**


Un problema real de datos sucios que cae seguido. Para detectarlos, **GROUP BY email HAVING COUNT(*) > 1** muestra qué valores se repiten. Para quedarte con una sola copia, la forma moderna es numerar cada repetición con **ROW_NUMBER() OVER (PARTITION BY email ORDER BY id)** y conservar solo la fila número 1. Saber los dos lados —detectar y limpiar— es lo que buscan.


#### **4️⃣ Descomponer con CTEs y subconsultas**


Cuando la lógica se complica, meter todo en una sola consulta la vuelve ilegible. Una CTE, con **WITH nombre AS (...)** , separa el problema en pasos con nombre, más fáciles de leer y de depurar. Los entrevistadores valoran que uses CTEs para ordenar una consulta compleja y que sepas cuándo una subconsulta correlacionada es la herramienta adecuada.


#### **5️⃣ Manejar NULLs sin que te arruinen la consulta**


Los NULL son la fuente número uno de resultados incorrectos silenciosos. Hay que saber que un NULL no responde a una comparación con el operador igual (para eso se usa IS NULL), que **COALESCE(comision, 0)** reemplaza los NULL por un valor por defecto y —la trampa que casi nadie ve— que un NOT IN sobre una subconsulta que contiene un NULL devuelve cero filas. Detectar ese detalle en plena entrevista te distingue de inmediato.


*Si quieres que este nivel de SQL sea parte de tu perfil profesional, aplica a la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=sqlDataScientistEntrevistaConsultas) *y aprende a resolver estos problemas sobre datos reales.* ⚡


## **Cómo prepararte, más allá de memorizar**


Quienes toman estas entrevistas coinciden en algo: no evalúan si recitas sintaxis, sino si **reconoces el patrón detrás del pedido y explicas tu razonamiento en voz alta** . Por eso conviene practicar sobre datos reales en lugar de memorizar respuestas, y llegar a la entrevista con el hábito de empezar por el objetivo de negocio, plantear los pasos y explicar las decisiones: muchas veces eso pesa más que llegar a la respuesta perfecta.


*📎 La mejor forma de entrenar ese reconocimiento de patrones es construir proyectos con datos reales; en el blog tenemos una selección de[7 proyectos de Data Science](https://blog.soyhenry.com/7-proyectos-de-ciencia-de-datos-que-aprenderas-en-nuestro-programa/) para practicar.*


## **En resumen**


- En una entrevista de datos, SQL evalúa si traduces una pregunta de negocio en una consulta correcta, no si memorizaste la sintaxis.
- Los cinco fundamentos ineludibles: JOINs (INNER vs LEFT), GROUP BY + HAVING, el segundo valor más alto, el ranking por grupo y GROUP BY vs funciones de ventana.
- Las cinco que marcan dominio: CASE WHEN, series temporales con LAG y acumulados, duplicados, CTEs y el manejo de NULLs.
- Las funciones de ventana ya son un requisito básico; conocer la diferencia entre ROW_NUMBER, RANK y DENSE_RANK es clave.
- Más que la respuesta perfecta, el entrevistador valora que reconozcas el patrón y expliques tu razonamiento.


## **Conclusión**


Prepararte para la parte de SQL de una entrevista de datos no es cuestión de memorizar cien consultas, sino de dominar un puñado de patrones que se repiten y de saber reconocerlos apenas escuchas el problema. Los joins, las agregaciones con su filtro, el ranking por grupo, las funciones de ventana sobre el tiempo y el manejo de NULLs cubren la enorme mayoría de lo que te van a pedir. Y lo que termina de convencer a quien te entrevista no es solo que llegues al resultado, sino que puedas explicar por qué elegiste ese camino.


*Si quieres construir esa base de SQL —y todo el stack de un perfil de datos— con proyectos reales, mentores de la industria y acompañamiento hasta conseguir empleo, la*[Carrera de Data Science de Henry](https://www.soyhenry.com/carrera-data-science?utm_source=blog&utm_medium=organico&utm_campaign=sqlDataScientistEntrevistaConsultas) *está diseñada para eso. Aplica y empieza a prepararte para tu primera entrevista de datos.* 🚀


## **Preguntas frecuentes**


**¿Qué nivel de SQL piden para un puesto junior de Data Science?**


Un nivel sólido de fundamentos: joins, agregaciones con GROUP BY y HAVING, subconsultas y, cada vez más, funciones de ventana. No se espera que optimices bases de datos como un ingeniero, pero sí que resuelvas con fluidez problemas de extracción y análisis, y que expliques tu razonamiento.


**¿Necesito memorizar la sintaxis exacta de cada consulta?**


No. La mayoría de las entrevistas aceptan SQL estándar y no penalizan diferencias menores entre PostgreSQL y MySQL. Lo que evalúan es tu lógica: reconocer el patrón del problema y estructurar la solución. Explicar tu enfoque en voz alta cuenta tanto como el código.


**¿Qué son las funciones de ventana y por qué las piden tanto?**


Son funciones como ROW_NUMBER, RANK, LAG o SUM() OVER (...) que calculan un valor sobre un conjunto de filas relacionadas sin colapsarlas en una sola. Se piden porque resuelven de forma elegante problemas muy comunes —rankings por grupo, acumulados, comparaciones temporales— y hoy se consideran una habilidad básica, no avanzada.


**¿Cómo practico SQL para entrevistas si no tengo experiencia laboral?**


Con datos reales: descarga datasets públicos, plantéate preguntas de negocio y resuélvelas con consultas. Construir proyectos propios donde extraes y analizas datos te da, a la vez, la práctica de SQL y algo concreto para mostrar y contar en la entrevista.


**¿En qué parte del proceso de selección aparece SQL?**


Casi siempre en la ronda técnica. Puede ser una sesión de live coding, donde compartes pantalla y explicas mientras escribes, o una prueba online con tiempo límite. Suele ser una de varias etapas del proceso: un filtro inicial, la parte técnica de SQL o Python, preguntas de estadística o machine learning y, en muchos casos, un caso de negocio.


### Etiquetas


- [Data Science](https://blog.soyhenry.com/tag/data-science/)
- [Trabajar en Tecnología](https://blog.soyhenry.com/tag/trabajar-en-tecnologia/)
