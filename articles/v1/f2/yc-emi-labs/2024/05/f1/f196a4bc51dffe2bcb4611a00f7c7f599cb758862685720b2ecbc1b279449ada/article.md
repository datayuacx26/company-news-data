---
schema_version: "1.0.0"
document_id: "f196a4bc51dffe2bcb4611a00f7c7f599cb758862685720b2ecbc1b279449ada"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-news-import-6c95cb972f69"
canonical_url: "https://www.emilabs.ai/es/blog/microservicios-1a-parte"
published_at: "2024-05-09T00:00:00+00:00"
first_seen_at: "2026-07-24T06:07:38.705314+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:59686d72d07ddb8ed52adaa74166781ba1d1ddf9bb9c0eeb139d13f9df0cbe78"
---

# Todo sobre los microservicios (1a parte)

En esta publicación, compartiremos cómo manejamos las decisiones de arquitectura y diseñamos nuestros microservicios en Emi, con la esperanza de que estas experiencias puedan ser útiles o servir de inspiración para otros. ¡Comencemos!


# ¿Por qué microservicios?


Los servicios deben ser construidos y mantenidos por equipos pequeños, diseñados en torno a capacidades comerciales (no en torno a capas horizontales como el acceso a datos o a mensajería) y autónomos (desplegados como un servicio aislado y modificados independientemente). Algunos de sus beneficios son:


- Alineación organizacional: Los equipos más pequeños son más productivos y tienen más autonomía.
- Facilidad de despliegue: Aísla los negocios, permitiendo lanzamientos más rápidos.
- Resiliencia: Permite aislar un problema para que el resto del sistema pueda seguir funcionando.
- Escalabilidad: Se pueden establecer diferentes políticas de escalabilidad y tipos de hardware para cada servicio.
- Componibilidad: La funcionalidad puede ser consumida de diferentes maneras.
- Optimización para la reemplazabilidad: Facilidad para reemplazar/refactorizar servicios.
- Heterogeneidad tecnológica: Cada servicio puede utilizar diferentes lenguajes de programación, bases de datos, herramientas, etc.


Pero nada es gratis en este mundo, y los microservicios no son la excepción. Este patrón arquitectónico viene acompañado de los mismos desafíos que cualquier solución distribuida o decisión arquitectónica y organizacional.


# ¿Por qué Node.js?


Node.js es un entorno de ejecución de JavaScript, y JavaScript es uno de los lenguajes más utilizados (principalmente porque se ejecuta en todos los navegadores). En las primeras etapas de Emi, decidimos construir la mayoría de nuestros servicios utilizando Node.js porque:


- Es fácil de aprender, y tiene una comunidad muy grande
- Ayuda con la programación asíncrona (mejor rendimiento y excelente para arquitecturas basadas en eventos)
- Su lenguaje dinámico ofrece una mayor velocidad al codificar, menos protocolo y buena facilidad para las pruebas
- Cuando haya necesidad de mezclar código dinámico con código fuertemente tipado, se podría utilizar algo como TypeScript


# Desafíos


Las comunicaciones resilientes entre servicios, la aceleración de la creación de servicios, la resolución de problemas comunes como configuraciones, el manejo de errores, el consumidor de mensajes y, por último pero no menos importante, el rastreo distribuido, son algunos de los desafíos inherentes a la arquitectura de microservicios.


En Emi, hemos construido y mantenemos activamente paquetes npm para ayudarles a aquellos que trabajan con microservicios en Node.js a resolver estos desafíos.


# Un momento, ¿no deberían ser autónomos los equipos y servicios?


Entendido. Sí, al seguir un conjunto de directrices, cada equipo puede decidir qué lenguaje y bibliotecas utilizar mientras cumplan con algunas reglas arquitectónicas, de modo que cada servicio sea resiliente, observable y escalable.


Al final, las reglas arquitectónicas y las mejores prácticas son la base para cada servicio dentro de un ecosistema de microservicios. Estos paquetes facilitan el cumplimiento de estas reglas, permitiéndonos avanzar rápidamente, guiados por una mentalidad orientada a la creación de valor.


# Comunicaciones resilientes entre servicios: timeout (tiempo máximo de ejecución) y reintentos


Con tantos servicios comunicándose entre sí, es fundamental que puedan tolerar fallas de otros componentes de los que dependen. Los dos patrones más comunes para lograr esto son el timeout y los reintentos.


En la comunicación mediante mensajería en espera (colas de mensajes), éstos son fáciles de implementar porque la infraestructura probablemente los manejaría de forma predeterminada. Pero, ¿qué sucede con nuestra comunicación HTTP? Bueno, el timeout probablemente sea manejado por la biblioteca del cliente HTTP, pero es importante que todos lo estén usando correctamente. Si el valor del tiempo de espera siempre es de 5 minutos (o está desactivado, como lo hace[Axios](https://github.com/axios/axios) ), es probable que esperes demasiado tiempo por servicios inactivos, y tus consumidores agotarán el timeout antes de que termine.


Por otro lado, la mayoría de los clientes HTTP no suelen manejar reintentos. Hemos construido un módulo HTTP que debe ser utilizado por cada servicio basado en Node.js. Este utiliza Axios, para el cual hemos modificado algunos valores predeterminados:


También hemos utilizado[axios-retry](https://github.com/softonic/axios-retry) , que intercepta las solicitudes fallidas y realiza reintentos siempre que sea posible utilizando una estrategia de retraso exponencial:


# Rastreo distribuido


El rastreo distribuido es crucial para entender aplicaciones complejas de microservicios. Sin él, puede que los equipos no logren identificar cuándo haya problemas de rendimiento u otros errores en su entorno de producción.


Imaginemos esto: Las tareas a menudo abarcan varios servicios. Cada servicio maneja una solicitud realizando una o más operaciones, como solicitudes HTTP, consultas a bases de datos y espera de mensajes. Dicho esto, para facilitar la comprensión y la depuración del comportamiento de una aplicación, uno debería poder rastrear todas las solicitudes y operaciones que pertenecen a la misma tarea.


Empaquetamos nuestros servicios con un middleware de express que asigna a cada tarea un identificador (ID) de rastreo único (UUID). Este ID está destinado a ser agregado en el primer servicio controlado que maneja una solicitud externa (por ejemplo, Backend para Frontend, API Pública) o donde se inició internamente la solicitud (por ejemplo, tarea cron). Este middleware también agrega el nombre del iniciador (servicio y componente) y el identificador de usuario conectado como contexto. Luego, los datos de rastreo y de referencia se pasan a todos los servicios que estén involucrados en el manejo de la operación, lo cual permite incluir los datos de rastreo en todos los mensajes de registro, métricas y respuestas de la API.


Es fácil leer un ID de rastreo o generar uno nuevo. Pero, ¿cómo pasar este ID a cada función en la cadena de llamadas que realizará nuestro proceso? No hay duda de que agregar este ID como un argumento en cada función no es una opción. Se necesita agregar algún "código de instrumentación" a nuestro código de negocio para rastrear estas trazas de manera transparente. Para resolver este problema en particular, optamos por el enfoque más sencillo y utilizamos[Continuation-Local Storage (Hooked)](https://github.com/jeff-lewis/cls-hooked) , un módulo que utiliza ganchos asíncronos y permite establecer y obtener valores que están vinculados al ciclo de vida de estas cadenas de llamadas de función 😉.


Aquí puedes ver un fragmento para darte una idea de cómo funciona este módulo:


Luego, en tu formateador de registros, puedes leer este ID de rastreo para después incluirlo en los registros:


Mantenemos módulos para registro, mensajería, un cliente HTTP y middlewares para APIs web y Workers, y todos están preparados para enviar y/o leer estos IDs de rastreo.


# Continuará…


Parece mucho, ¿verdad? Está bien sentirse un poco abrumado si es la primera vez que te enfrentas a los microservicios. Es un tema difícil y, como pasa con cualquier tema difícil, lleva tiempo y práctica dominarlo.


La próxima semana, abordaremos otros temas como configuraciones, plantillas de microservicios y linters (analizadores de código). Aún hay mucho por aprender; ¡mantente atento!


Y si has llegado hasta aquí, podría ser una buena idea echar un vistazo a las[ofertas de trabajo](https://jobs.lever.co/emilabs) que tenemos actualmente. ¡Estamos buscando personas apasionadas y curiosas para unirse a nuestro equipo!


‍


‍
