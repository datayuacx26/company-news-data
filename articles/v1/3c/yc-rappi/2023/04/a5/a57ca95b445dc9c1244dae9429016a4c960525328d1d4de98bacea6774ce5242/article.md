---
schema_version: "1.0.0"
document_id: "a57ca95b445dc9c1244dae9429016a4c960525328d1d4de98bacea6774ce5242"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/ios-arquitecturas-de-aplicaciones-37c3c7f4d094"
published_at: "2023-04-12T22:58:35+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:4de74fe44a7b5235b01e4f4041c2aae9bbd09e3d71f9212927f96e03d87c840c"
---

# iOS: Arquitecturas de aplicaciones

IOS


Architecture


Architecture Pattern


# iOS: Arquitecturas de aplicaciones


[FrankGumeta](https://medium.com/@frankgumeta?source=post_page---byline--37c3c7f4d094---------------------------------------)


10 min read


·


Feb 10, 2023


--


Press enter or click to view image in full size


[gdtography](https://www.pexels.com/es-es/foto/fotografia-de-larga-exposicion-interior-de-edificio-de-domo-blanco-911758/)


2023 ya llegó, y vino con más trabajo, nuevas cosas que aprender y muchas nuevas preguntas. Las más habituales referente a arquitectura suelen ser: **¿Que arquitectura debería escoger para mi aplicación?** , **¿Cual es la mejor de todas?** , **¿Existe una arquitectura que pueda usar para todo?**


No te preocupes, aquí encontraremos respuesta a todas estas pregunta y con suerte, aprenderemos a crear código más estructurado. Agarra tu recalentado de fin de año, toma lo que sobró del ponche con piquete y ponte cómodo. Hoy vamos a aprender todo lo que necesitamos saber sobre arquitecturas de aplicaciones para iOS.


## El inicio


¿Por dónde iniciamos?, por el inicio bro, lo dice en el título de la sección, ponte vivo porque distraído no me sirves.


Entonces, comencemos preguntándonos:


## ¿Que es una arquitectura?


Podríamos definirlo como:


> El conjunto de principios y decisiones de diseño que se utilizan para estructurar y organizar el código de una aplicación . La arquitectura de una aplicación móvil puede incluir aspectos como la estructura de carpetas y módulos, la forma en que los componentes de la aplicación se comunican entre sí y la forma en que se manejan los datos y la lógica de negocio.


Te lo voy a poner de una manera más sencilla.


Al igual que una casa necesita tener una base sólida, una serie de paredes y habitaciones organizadas de manera lógica, una aplicación necesita tener una arquitectura sólida y bien pensada para poder soportar su propio peso (es decir, el código y los datos que contiene).


Vamos, son distintas reglas para construir. Por eso siempre he pensado, que los desarrolladores somos fundamentalmente albañiles informáticos.


Tal vez te estés preguntando porque necesitas esto, ya eres un excelente programador y tu código siempre es pulcro y organizado.


Seguir una arquitectura te permite **estandarizar** los cimientos de tu app, para que tú y tus compañeros de equipo, y el becario que contraten cuando te despidan puedan seguir el mismo patrón de desarrollo haciendo que tu aplicación sea estable y flexible a lo largo del tiempo


Ahora que entendemos que es una arquitectura, viene la pregunta del millón:


## ¿Qué arquitectura es la mejor?


Si me dieran 1 dolar por cada que he escuchado el monólogo de un dev explicar porque la arquitectura que acaba de aprender es la mejor de todas, facilmente podria comprar medio Rappi sin problemas y jubilarme ya mismo.


Te voy a adelantar la respuesta: **DEPENDE**


Ya se, ¿cómo es posible que no haya una arquitectura superior a todas?


Pues sí, mi estimado padawan. Así como no creas una cabaña de concreto sólido con elevadores para 1 solo piso, tampoco creas un edificio de 20 niveles con madera de pino. Bueno, nunca falta quien si, pero obviamente no es lo correcto.


Para poder saber qué arquitectura es mejor para nuestra aplicación, primero tenemos que conocer algunas de ellas.


## ARQUITECTURAS


Tranquilo, no listare todas las que existen, y tampoco entraré en detalle a cada una de las que listaré, podría hacer un artículo por cada una, pero no me pagan lo suficiente, así que en este artículo te mostraré las características principales, ventajas y desventajas. Si te gustaría ver a detalle cada una de ellas. Dejame un comentario o señal de humo y puede ser que haga más articulos para hablar de cada una de ellas.


## MVC — Modelo Vista Controlador


Press enter or click to view image in full size


[Apple](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)


### Partes


**Modelo** : representa los datos y la lógica de negocio de la aplicación.


**Vista** : se encarga de la interfaz de usuario y de mostrar los datos del modelo.


**Controlador** : actúa como intermediario entre la vista y el modelo, recibiendo acciones del usuario a través de la vista y actualizando el modelo en consecuencia. También puede actualizar la vista para reflejar cambios en el modelo.


### **Ventajas**


- Sencillo de aprender


### **Desventajas**


- La vista puede depender mucho del modelo y del controlador, lo que puede dificultar la reutilización de vistas.
- El controlador puede acabar siendo demasiado grande y complejo si se le asignan demasiadas responsabilidades (no por nada le llaman **Massive View Controller** ).


### Bueno para


- Aplicaciones muy, muy sencillas, pruebas de concepto, o como punto de partida para aprender sobre arquitecturas. No te lo recomendaria para aplicaciones de nivel profesional


### Nota


En resumen, la arquitectura MVC funciona dividiendo la aplicación en tres componentes: el modelo, que representa los datos y la lógica de negocio; la vista, que se encarga de mostrar los datos de manera adecuada para el usuario; y el controlador, que actúa como intermediario entre la vista y el modelo y se encarga de actualizar uno y otro en consecuencia.


## MVVM — Model View ViewModel


Press enter or click to view image in full size


[Farhana Mustafa](https://blog.devgenius.io/using-the-mvvm-architectural-design-pattern-in-ios-c70e16352be5)


### Partes


**Modelo** : representa los datos y la lógica de negocio de la aplicación.


**ViewModel** : es una clase intermedia que actúa como intermediario entre la vista y el modelo. Se encarga de proporcionar los datos que necesita la vista para mostrarlos al usuario y de recibir acciones del usuario a través de la vista y actualizar el modelo en consecuencia.


### Ventajas


- Relativamente sencillo de aprender
- Desacopla la lógica de las vistas de la de negocio
- Es una excelente opción para ambientes donde se usa programación reactiva


### Desventajas


- El ViewModel puede depender mucho del modelo y de la vista, lo que puede dificultar la reutilización de ViewModels.
- La vista puede depender mucho del ViewModel, lo que puede dificultar la reutilización de vistas con otros view models.


### Bueno para


Desacoplar lógica de las vistas, en general si tu app tiene muchas vistas, es buena idea utilizar esta arquitectura. No por algo es la elección número 1 cuando utilizamos **SwiftUI** . Y de hecho, **se lleva bien con programación reactiva** ya que la idea principal detrás de esta arquitectura es que los cambios de estado se propagan automáticamente a las vistas.


### Nota


No te dejes llevar porque el diagrama se vea muy parecido (por no decir igual) que en MVC, el código en esta arquitectura es muy diferente. Una vez más, si quieres ver a detalle cada una de estas, dejame un comment para saber que te interesa conocer a detalle estas arquitecturas


## VIPER — View Interactor Presenter Entity Router


[Bipin Pandey](https://medium.com/cr8resume/viper-architecture-for-ios-project-with-simple-demo-example-7a07321dbd29)


### Partes


**Interactor** : se encarga de la lógica de negocio de la aplicación y de procesar los datos del modelo. En palabras sencillas, este componente es el que trae los datos a través de un service.


**Presentador** : es una clase intermedia que actúa como intermediario entre la vista y el interactor. Se encarga de procesar la información del interactor y prepararla para su visualización en la vista, así como de recibir acciones del usuario a través de la vista y enviar intenciones al interactor para actualizar los datos en consecuencia.


**Entity** : representa los datos del modelo.


**Router** : se encarga de la navegación entre las diferentes pantallas de la aplicación.


## Get FrankGumeta’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Service** : se encarga traer la información que el módulo necesita (servicios web, base de datos, etc…) y mapearla a sus respectivas clases/estructuras, su entidad pues


### Ventajas


- Facilita la separación de responsabilidades y la reutilización de código.
- Permite una mayor flexibilidad y extensibilidad al permitir la creación de nuevos modelos y vistas sin tener que modificar el presentador.
- Hace relativamente facil testear (casi) todos sus componentes


### Desventajas


- Puede ser difícil entender cómo interactúan entre sí los diferentes componentes de la aplicación.
- El presentador y el router pueden depender mucho del interactor y de la vista, lo que puede dificultar la reutilización de estos componentes
- No es tan amigable con programación reactiva, podría llevarte a escribir mucho boilerplate si no tienes cuidado


### Bueno para


Proyectos grandes que no usan programación reactiva. Esta arquitectura te permite reutilizar en su totalidad cualquier componente de las vistas. El tráfico de datos siempre pasará por el presenter y es donde deberás tener cuidado de no sobrecargar a la hora de diseñar tus módulos.


### Nota


VIPER es probablemente una de las arquitecturas más populares en desarrollo para iOS, es facilmente escalable, hace sus componentes relativamente fáciles de testear, pero no te confies, si no tienes cuidado con lo que haces, puedes terminar con módulos sobrecargados con cientos de lineas de codigo por cómo organiza su flujo de datos (nótense las flechas que unen los componentes).


## CLEAN


[Kamyar Elyasi](https://betterprogramming.pub/quick-clean-swift-4955d8ac6788)


### Partes


**Vista** : se encarga de la interfaz de usuario y de mostrar los datos del modelo


**Interactor** : Al igual que en VIPER, es el encargado de la lógica de negocio, entiéndase ir por la información que se necesita a donde sea que esta este a través de Workers


**Worker** : Es el encargado de traer información y mapearla.


**Presenter** : Recibe la información del interactor, se encarga de formatearla de manera adecuada antes de enviarla al controller


**Controller** : Se encarga de procesar la información del modelo y prepararla para su visualización en la vista, así como de recibir acciones del usuario a través de la vista y actualizar el modelo en consecuencia.


**Modelo** : representa los datos y la lógica de negocio de la aplicación.


### Ventajas


- Es tan modularizable como VIPER ya que comparten prácticamente los mismos componentes.
- Es más fácil de testear que VIPER


### Desventajas


- Un developer nuevo podrá crear fácilmente retain cicles si no nota la referencia circular del flujo de datos (en un momento hablaré de eso).
- No es tan amigable con programación reactiva :C


### Bueno para


Proyectos de medianos a grandes, que requieran testear tanto código como requieran, y por su alta capacidad de modularización y su forma de interconectarse, es mas sencillo de testear que VIPER.


### Nota


Mucha comparación con VIPER ¿no?, es obvio, comparten básicamente los mismos componentes y hacen casi lo mismo, pero no se conectan entre sí de la misma manera, así como unas botas y unos tenis comparten las mismas partes que las forman no se amarran ni se ven igual ¿cierto?.


Personalmente Swift Clean (dejemoslo asi, porque CLEAN no es una arquitectura, es un estilo de desarrollo) es de mis favoritas porque es lo suficientemente modular para hacerlo altamente testeable y lo suficientemente sencillo para comprenderlo fácilmente.


## TL;DR


Press enter or click to view image in full size


Ya llévame diosito


¿Cansado?, y eso que no listamos otras arquitecturas como **RIB’s, Coordinator, MVI, Flux, Redux** y otras menos conocidas como **MVCVS** o **MAVB** . (Ya sabes, dejame un mensaje si quieres conocer alguna a fondo, soy developer no adivino).


En fin, no olvidemos la razón por la que estamos aquí.


Entonces, **¿cuál elegir para mi proyecto?**


Una vez mas…


## Depende


Quitando la eterna guerra entre los Arquitectos e Ingenieros civiles. Cuando un Arquitecto Civil diseña un edificio, lo hace pensando en **las características que va a tener el edificio** , basa su diseño en **cómo organizar la estructura para que este no se caiga** , y pueda ser **tanto bello como funcional** .


En desarrollo de software tenemos un reto **similar** , tenemos que escoger la mejor manera de construir nuestros sistemas **pensando en las características que nuestras apps van a tener** , tenemos que pensar cómo **organizar todos los datos y los módulos que lo compondrán** , y tenemos un reto adicional, tenemos que pensar como construir nuestros sistemas **pensando en que estos van a crecer** a lo largo del tiempo.


Muchas veces también tenemos que pensar en el stack de tecnología que nuestros equipos conocen, pensar en los costos de transicionar de una arquitectura a otra cuando la tecnología de obliga (Si no ahi estamos de ejemplo en Rappi moviendonos de UIKit a SwiftUI).


Press enter or click to view image in full size


Este artículo me confundió mas


Este artículo no pretende decirte que arquitectura tomar, ni cual es la mejor de todas, porque depende del tipo de proyecto que vas a construir. Pero si aun necesitas responderte cual usar, lee lo siguiente:


Si tu proyecto y tu equipo es:


### Grande, Muy Grande


Tienes que priorizar


- Testabilidad
- Modularización
- Performance
- Flexibilidad


Te recomiendo que evalues a fondo: VIPER, SWIFT CLEAN, RIB’s, MMVM


### Mediano


Tienes que priorizar


- Velocidad de desarrollo
- Testabilidad


Ojito, no significa que puedes olvidarte del performance y la modularización. Trato de decirte que en aplicaciones de mediano tamaño, podría ser esto tenga menor prioridad que la velocidad de desarrollo.


Te recomiendo evaluar: SWIFT CLEAN, MVVM.


### Pequeño


Aquí no hay mucho que priorizar más allá de que tu app sea escalable en caso de que comience a tener cada vez más usuarios. Repito, tampoco significa que debas olvidarte de todo lo demás.


Te recomiendo evaluar: SWIFT CLEAN, MVC (Solo si son pruebas de concepto o cosas que no van a crecer después).


## Conclusión


Una vez más, escoger una arquitectura para iniciar o transicionar un proyecto nunca es una tarea fácil. Tienes que tomar muchos factores en cuenta. Que si el stack de tecnología, que si los devs de la empresa no conocen tal o cual herramienta, que si el jefe quiere la app para mañana pero quiere que sea escalable y flexible, que si hay que poner 8952 pantallas y animaciones. En fin, ser Arquitecto de Software nunca es fácil.


Afortunadamente tenemos estas arquitecturas, guías y principios (SOLID estas ahi?) que nos facilitan un poco el trabajo. Pero recuerda, **una app, una arquitectura y nuestro código, son tan buenos como seamos programando** . Recuerda también que ella no te ama y que hay muchas más arquitecturas por ahí flotando. **Revísalas** , **compréndelas** , y eventualmente entenderemos que **cada herramienta tiene su propia utilidad.**


Espero que este artículo te haya servido para al menos ayudarte a como decidir sobre qué arquitectura utilizar, y con suerte conocer la existencia de alguna que otra arquitectura.
