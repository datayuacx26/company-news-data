---
schema_version: "1.0.0"
document_id: "0b63b049471007949b56fd8fdc602a2fe1517060861072822bd277e4b6debe99"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/mi-primer-a%C3%B1o-como-cient%C3%ADfico-de-datos-b7211fc1c7f8"
published_at: "2022-09-15T21:38:06+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-08-20T02:17:23.302365+00:00"
content_hash: "sha256:bbd5653c398453f813af6da1c1104aaea3f50e64cf26df18b60a578373880485"
---

# Mi primer año como científico de datos

Es cierto que trabajar en la industria de la inteligencia artificial no es una tarea fácil, las necesidades se transforman, los requisitos de los interesados cambian de un momento a otro, y por supuesto… el cambio es la única constante.


Inicié en Rappi como científico de datos junior en junio del 2021, siendo esta una posición bastante flexible, llena de retos, y sobre todo… con mucho trabajo por hacer.


Durante todo este tiempo, he estado trabajando en una variedad bastante amplia de proyectos, lo que me ha permitido aprender de muchas ramas del conocimiento, y sobre todo, de compañeros más experimentados que yo.


Teniendo todo esto en mente, he escrito este pequeño artículo en español, con la finalidad de ayudar a todas las personas que recientemente empiezan su carrera profesional en este ámbito, y platicarles de las cinco cosas que a mí me hubiera gustado saber al momento de iniciar este viaje.


#### **1. Definir el problema es la mejor manera de empezar**


Existen ocasiones, en las que se nos presenta un nuevo reto con un solo requisito: *“Desarrollar un modelo de inteligencia artificial, que nos ayude a <inserte necesidad de la siguiente lista>”* .


- Detectar fraude
- Maximizar ventas
- Aumentar la cartera de clientes
- Retener talento


Sea cual sea la necesidad que elegiste, tener como único requisito, *“Desarrollar un modelo de inteligencia artificial, que nos ayude a detectar fraude”* , no es suficiente.


Aunque “fraude” es una palabra que la mayoría del mundo tiene en su vocabulario, ten por seguro, que la definición de “fraude” va a ser diferente para la persona que necesita de tu apoyo.


Por lo que es absolutamente importante, realizar todas las preguntas necesarias para empezar a definir tu proyecto, y sobre todo: definir el objetivo, el alcance, y las métricas de éxito para tu modelo.


**¡Spoiler alert!**


> “Posiblemente te enteres en el camino, que el problema principal nunca fue el que hablaron en primer lugar”


Photo by[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Algo que hay que tomar en cuenta, es que para hacer las preguntas correctas, tienes que conocer del tema… lo que me lleva a mi siguiente punto.


#### **2. Entender los datos es de vital importancia**


Como científico de datos, lo más sencillo y rápido que podemos hacer, es recopilar todos los datos disponibles sin analizarlos, con la finalidad de entrenar el modelo más complejo y robusto que tengamos a nuestra disposición y esperar que el modelo encuentre las relaciones que expliquen de la mejor manera el comportamiento de la variable objetivo.


Este es un gran error, debido a que los algoritmos de machine learning van a encontrar relaciones incluso en dónde no las hay.


La figura 1 muestra la relación entre el consumo per cápita de queso mozzarela y los doctores recién graduados en ingeniería civil. Como podemos observar, existe una correlación bastante fuerte… ¿Será acaso, que los doctores en ingeniería civil aumentan el consumo per cápita del queso mozzarella?


> ¡La respuesta es un rotundo No!


Figura 1: Imagen obtenida de[https://www.tylervigen.com/spurious-correlations](https://www.tylervigen.com/spurious-correlations)


Esta es la gran diferencia entre correlación y causalidad. Un evento puede estar correlacionado con tu variable objetivo, sin la necesidad de que uno implique al otro en la vida real. Lo que puede ocasionar problemas de rendimiento en producción, y sobre todo, problemas de interpretabilidad al momento de entender y explicar el funcionamiento de nuestro modelo con los stakeholders.


Si no analizamos a profundidad nuestros datos, esta y muchas cosas más pueden ocurrir. Algunas otras historias de terror que pueden pasar son:


- Valores faltantes debidos a inconsistencias con la ingestión de la data
- Dependencias temporales de nuestra variable objetivo con eventos externos
- Cambios abruptos de la variable objetivo, debido a cambios en la definición de la variable por parte de negocio
- Sesgos humanos en los datos


Todo esto podría estar pasando, y la única forma de saberlo, es ponernos manos a la obra y entender los datos desde su origen.


#### **3. La solución más simple con mejores resultados siempre es la mejor**


Existen ocasiones en las que es necesario brindar una solución oportuna para algún problema en específico, por lo que tu equipo y tú han planteado algunas soluciones bastante interesantes que pudieran resolver el problema.


Dentro de esas soluciones existen algunas que son muy complejas tanto en cuestión de implementación como en desarrollo, como hay otras que no lo son tanto. La pregunta es: ¿con cuál deberían de empezar?


> La respuesta es: ¡Depende!


Cada solución que nosotros proponemos va a tener sus propios beneficios e inconvenientes. Sin embargo, algo que es 100% seguro, es que **la solución más simple con mejores resultados siempre es la mejor** .


Tenemos que reconocer que nuestro trabajo está 100% conectado con otras áreas de la empresa, que al igual que tú, hacen su mejor esfuerzo para llevar su producto o servicio a otro nivel. Por lo que cualquier solución que nosotros implementemos, va a requerir en menor o mayor medida, la participación de: negocio, machine learning engineers, data engineers, data analysts, entre muchos otros más. Con lo que, entre más compleja sea la solución, existe una mayor probabilidad de que algo falle.


Photo by[Jud Mackrill](https://unsplash.com/@judmackrill?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Así que la próxima vez que tengas que resolver un problema con inteligencia artificial, pregúntate primero, si realmente es necesario entrenar un modelo.


> Tal vez te lleves la sorpresa, que realmente no es necesario, y que con una simple regla se soluciona a la perfección el problema.


#### 4. **Si lo que desarrollaste no tiene ningún impacto, realmente no hiciste nada**


La primera vez que eres nombrado como científico de datos, una de las cosas que más anhelas, es hacer el modelo más complejo jamás hecho. Por lo que decides aventurarte con redes neuronales del estado del arte. No obstante, en el camino te das cuenta de que lo que desarrollaste nadie lo está usando… Por lo que te pones a investigar, y te das cuenta de lo siguiente: la solución fue tan compleja que los tiempos de respuesta excedían el límite permitido, que el modelo no era lo suficientemente robusto a las pequeñas perturbaciones, con lo que el rendimiento en producción era deficiente, y solo por empeorar las cosas, la explicabilidad del modelo a cada observación no tenía ningún sentido para negocio.


Con esto, no estoy diciendo que siempre necesitamos tener modelos con resultados inmediatos o que jamás tenemos que explorar modelos del estado del arte. Lo que sí me gustaría enfatizar, es que, si nuestra intención es contribuir a la solución de un problema, nuestro esfuerzo se ve reflejado en qué proporción lo estamos resolviendo, o siendo más técnicos, qué tanto nuestra solución mejora la métrica de negocio.


Photo by[Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Por lo que antes de empezar, como ya vimos, es necesario que tengamos bien definido cuál es la métrica de negocio que tenemos que mejorar, y con base en eso, proponer la solución que mejor solucione los requisitos del cliente.


#### 5. **Código limpio y basado en funciones**


El trabajo de un científico de datos por su naturaleza tiene una componente creativa bastante fuerte, lo que hará que realicemos una infinidad de experimentos durante toda nuestra vida profesional. Lo más grandioso de esto, es que cada experimento que nosotros hagamos, ya sea exitoso o no, contribuirá en nuestra experiencia como profesionales, preparándonos así, para nuevos retos que posiblemente se puedan solucionar con la experiencia de todos los intentos fallidos.


> Teniendo todo esto en consideración, ¡no dejes un intento fallido en el cajón del olvido!


Cada vez que trates de resolver un problema con código, trata de que tu código esté basado en funciones, y que cada función realice una tarea en específico, de tal forma, que puedas recurrir a ellas cuando lo necesites. Siempre preguntándote, si alguien más sería capaz de entender tu código sin ningún contexto, y sobre todo, si tú lo entenderías tras cinco días, un mes o incluso todo un año sin haberlo visto.


Photo by[Artturi Jalli](https://unsplash.com/@artturijalli?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Si empezamos a adoptar buenas prácticas de programación, nuestra vida se hará más fácil, dado que, cada experimento contribuirá no solo a tu experiencia, sino también a tu biblioteca personal… y así mi estimado lector, cuando te enfrentes a un nuevo proyecto, tendrás una rotunda ventaja… no tendrás que empezar desde cero.


Mi nombre es **Víctor Maya** , y al igual que tú, me encanta resolver problemas con inteligencia artificial. Si tienes cualquier duda o comentario no dudes en contactar conmigo, me gustaría saber tu opinión.


Ten un excelente día y que los datos te acompañen 😉.


[LinkedIn](https://www.linkedin.com/in/victormayas/)


---


[Mi primer año como científico de datos](https://engineering.rappi.com/mi-primer-a%C3%B1o-como-cient%C3%ADfico-de-datos-b7211fc1c7f8) was originally published in[Rappi Tech](https://engineering.rappi.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
