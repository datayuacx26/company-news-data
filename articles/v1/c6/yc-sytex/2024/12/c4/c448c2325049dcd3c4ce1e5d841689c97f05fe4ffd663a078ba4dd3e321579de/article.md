---
schema_version: "1.0.0"
document_id: "c448c2325049dcd3c4ce1e5d841689c97f05fe4ffd663a078ba4dd3e321579de"
company_key: "yc-sytex"
company: "Sytex"
source_id: "yc-sytex-news-import-102764bfb914"
canonical_url: "https://sytex.io/post/in-defense-of-the-monolith"
published_at: "2024-12-13T00:00:00+00:00"
first_seen_at: "2026-07-26T07:48:27.861098+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:73d3182078f8a27f3696716b7d4180561d292f7d268ac10cb2da2f350fd3a2db"
---

# Why We're Sticking with the Monolith (For Now)

Sytex es un monolito de[Django](https://www.djangoproject.com/) . Es una base de código de más de un millón y medio de líneas. Las primeras líneas se escribieron cuando las[arquitecturas de servicios](https://martinfowler.com/articles/microservices.html) apenas empezaban a llamar la atención. Decidimos que Sytex seguirá siendo un monolito (por ahora), y aquí está nuestro razonamiento.


**Presten atención al “por ahora”**


En todos los aspectos de Sytex, entendemos que hay herramientas adecuadas para cada momento. Por ejemplo, nos apoyamos en nuestra base de datos relacional para las consultas de texto hasta que los beneficios de una base de datos de búsqueda se hicieron evidentes.


Como startup que apunta al crecimiento, tenemos que estar atentos al momento adecuado para iterar. Todo lo que sigue corresponde al estado actual de Sytex, y es probable que lo revisemos y posiblemente lo cambiemos en el futuro.


**Nuestros tres focos: producto, producto, producto**


Aunque llevamos varios años operando, tenemos clientes destacados y buena tracción, todavía nos consideramos[pre-PMF](https://a16z.com/12-things-about-product-market-fit/) . Por lo tanto, nuestro foco principal es iterar el producto hasta encontrar lo que nuestros clientes realmente quieren. Nuestros esfuerzos están dirigidos ahí, y hemos notado que cuando desviamos esfuerzo a evaluar nuevas tecnologías, perdemos foco. Rendimos mejor usando tecnologías “aburridas” que conocemos a fondo. De esta manera, podemos destinar todo nuestro presupuesto de incomodidad y nuestras ganas de desafiarnos a avanzar el producto. Exploramos nuevas tecnologías solo cuando el producto lo exige o cuando la escala lo requiere.


**Somos un equipo compacto al que le gusta moverse rápido**


Cuando seamos 100 ingenieros, el monolito probablemente se interpondrá en nuestro camino, y será[necesario desmantelarlo](https://martinfowler.com/bliki/MonolithFirst.html) **.** Lo tenemos presente y nos esforzamos por mantener un código testeable y modular. Pero en este momento, encontramos que las ventajas de mantener una arquitectura monolítica superan a sus desventajas. Entendemos que toda decisión técnica implica trade-offs, e intentamos evaluar qué características nos benefician o perjudican en cada paso. Nos mantenemos atentos para iterar cuando llegue el momento.


**Nos hemos equivocado**


Tenemos esta claridad ahora después de explorar alternativas y darnos cuenta de que lo que creíamos que nos aceleraría en realidad nos frenaría. Somos curiosos y nos gusta experimentar. Cada experimento nos enseña algo nuevo. Algunas lecciones permean nuestra base de código, mejorando nuestras prácticas. Otras nos muestran que un camino en particular no es el correcto (por ahora).


**En concreto** Lo que nos gusta:


- Tener una única tecnología de backend con la que todo el equipo se siente cómodo.
- Operaciones unificadas, entendidas por todos los desarrolladores. Las configuraciones son claras para todos.
- Es fácil levantar un entorno local muy similar al de producción.
- Iterar sobre las funcionalidades rápidamente sin introducir nuevas “incógnitas”.
- El stack de Python/Django no es el más eficiente del mercado, pero escalar la capacidad de cómputo es trivial.


Lo que no nos gusta:


- Los despliegues pueden volverse grandes, aumentando el riesgo de interrupciones del servicio.
- Los tiempos de integración aumentan de forma aproximadamente lineal con la cantidad de funcionalidades agregadas.
- No podemos escalar funcionalidades específicas de forma aislada, lo que eleva los costos operativos.
