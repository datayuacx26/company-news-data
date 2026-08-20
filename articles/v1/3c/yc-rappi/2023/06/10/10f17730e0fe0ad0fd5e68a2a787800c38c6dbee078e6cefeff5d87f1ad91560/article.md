---
schema_version: "1.0.0"
document_id: "10f17730e0fe0ad0fd5e68a2a787800c38c6dbee078e6cefeff5d87f1ad91560"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/core-mobile-en-rappi-38fec7d04c3e"
published_at: "2023-06-30T18:58:19+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:014fdd897148cae6fba92dbf556eb85ab0dd5eefe771364f7d0910220ce06600"
---

# Core Mobile en Rappi

# Core Mobile en Rappi


[Romel](https://medium.com/@romelapj?source=post_page---byline--38fec7d04c3e---------------------------------------)


8 min read


·


Jun 30, 2023


--


En Rappi buscamos seguir una filosofía Mobile First. Trabajando para mejorar significativamente la satisfacción y lealtad de nuestros usuarios, brindando una experiencia de usuario más rápida, intuitiva y fácil de usar en dispositivos móviles.


En los primeros años y con un equipo de desarrollo pequeño, era responsabilidad de todos garantizar que las aplicaciones móviles que teníamos en Rappi fueran técnicamente sólidas, escalables y seguras, además de que estuvieran diseñadas de acuerdo a las mejores prácticas de la industria.


Para mantenernos al día con las demandas de nuestros usuarios, las tendencias del mercado y el crecimiento de la empresa, fue crucial la construcción de un equipo core en el área móvil que fuera dedicado y especializado.


En este artículo quiero contarles en detalle la importancia que ha tenido en Rappi tener al equipo de **Core Mobile** , y presentarles las tareas y responsabilidades clave de este equipo. También discutiremos algunas de las tendencias emergentes en el desarrollo de aplicaciones móviles y cómo este tipo de equipos puede ayudar a una empresa a mantenerse al día con estas tendencias y tener una ventaja competitiva.


## ¿Cómo iniciamos?


Aprovechando que la empresa empezó a promover las encuestas de NPS ([Net Promoter Score](https://es.wikipedia.org/wiki/Net_Promoter_Score) ) para medir la satisfacción y la lealtad de los usuarios, decidimos que era importante antes de iniciar, hablar con nuestros usuarios. Y en el caso del equipo de Core Mobile, nuestros usuarios eran los desarrolladores móviles.


En el equipo usamos una encuesta de NPS para medir la satisfacción de los desarrolladores móviles y el valor que estaban recibiendo de las herramientas que utilizaban. Los resultados de la encuesta ayudaron al equipo a identificar áreas de mejora y a priorizar las características y funcionalidades que más importaban a nuestros usuarios.


Aquí algunas de las preguntas que el equipo realizó:


- ¿Cuál crees que es la mayor oportunidad de mejora para acelerar los tiempos en el[ciclo de desarrollo](https://codeclimate.com/blog/software-engineering-cycle-time/) ?
- ¿Qué es lo que más te frustra en el momento de estar desarrollando un feature?
- ¿Qué cambio sugerirías para mejorar nuestro día a día como desarrolladores y aumentar nuestra productividad en Rappi?


## Objetivos aspiracionales


Sabiendo cuales eran las principales preocupaciones de nuestros desarrolladores, decidimos crear objetivos aspiracionales como una herramienta valiosa para establecer una visión clara y definir un camino hacia el éxito dentro de la empresa. Decidimos definir metas desafiantes pero alcanzables que inspirarían al equipo a superarse y alcanzar su máximo potencial.


Desde entonces, estos objetivos han sido una herramienta valiosa, ya que nos ayudaron a alinear las metas del equipo con la estrategia de la empresa, motivar al equipo, mejorar su rendimiento y fomentar la innovación y la creatividad.


Los objetivos que nos planteamos fueron:


- Ofrecer una aplicación móvil de **clase mundial** .
- Crear una plataforma escalable y resistente que permita el **crecimiento de la empresa.**
- Garantizar una **plataforma de desarrollo rápida** .


## Equipos


Decidimos dividir el equipo en 3 squads con objetivos claros y definidos, cada uno enfocado en cumplir metas específicas que nos habíamos propuesto.


Los tres squads fueron:


- Arquitectura
- Tooling
- Design System


A continuación, les brindaré información detallada sobre cada uno de los squads, sus respectivos proyectos y algunos de los artículos que han creado para compartir con la comunidad.


## Squad de Arquitectura


El squad de Arquitectura se enfoca en el desarrollo, mantenimiento y mejora de la arquitectura de la aplicación de Rappi, así como en brindar soporte para librerías internas compartidas y “SDKs building capabilities”. Su objetivo es aumentar la eficiencia de los desarrolladores y mejorar la calidad de la aplicación.


Además, este squad ayuda a los ingenieros en la toma de decisiones de diseño y se asegura de que la arquitectura se mantenga limpia y en línea con la visión del equipo.


Dentro de sus responsabilidades, podemos encontrar:


- Diseñar, desarrollar y mantener la arquitectura de la aplicación.
- Mejorar la productividad de los desarrolladores mediante la creación de librerías y herramientas internas.
- Mejorar la calidad de la aplicación mediante la creación de librerías y guidelines internos.
- Reducir el tamaño de la aplicación a través del uso de herramientas y la arquitectura adecuada.
- Gestionar las dependencias en la aplicación.


## Proyectos recientes


En los últimos meses, el squad de Arquitectura ha estado trabajando en varios proyectos. Algunos de ellos incluyen:


### Nuevas developer tools


La construcción de nuestro command line tool, RAK (Rappi Army Knife), se encuentra en constante desarrollo y se agregan nuevas funciones periódicamente. Estas herramientas permiten mejorar el developer experience, tener una plataforma de desarrollo escalable (un ejemplo es la[arquitectura modular](https://engineering.rappi.com/modular-ios-architecture-at-rappi-interface-modules-327b44f723e4) ) y facilitan la adopción de nuevas tecnologías.


**Los tools permiten:**


- Crear módulos + configuración + templates
- Desarrollar nuevas pantallas con SwiftUI
- Generar proyectos enfocados en un solo módulo
- Reducir del tiempo de compilación


Press enter or click to view image in full size


Press enter or click to view image in full size


### Eliminación de módulo base


En el comienzo de Rappi, se había creado el módulo base para compartir clases entre los diferentes aspectos de la aplicación, como mercados y restaurantes. Sin embargo, a medida que la aplicación crecía y se añadían más funciones, el módulo base se hacía cada vez más grande y no había un equipo claramente responsable de él.


## Get Romel’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


El módulo base llegó a contener más de 9,500 líneas de código y era utilizado por casi 200 otros módulos. Las clases en el módulo base tenían diferentes propósitos, algunas eran para la lógica de negocio común, otras para la interfaz de usuario o para la inyección de dependencias.


Press enter or click to view image in full size


Con la eliminación del módulo base, el equipo logró reducir el acoplamiento entre los módulos, mover el código y recursos a los módulos donde se utilizan, disminuir el impacto al realizar un cambio en algún módulo, eliminar más de 100 recursos que no se utilizaban, disminuir en casi 2 MB el peso de la aplicación y eliminar un 24% del código sin utilizar.


### Live Activities


Este squad contribuyó en dar soporte a nuevas funcionalidades de las tiendas como los *Live Activities* y *Dynamic Island* . Este desarrollo mejoró la manera en la que los usuarios pueden hacer seguimiento de sus órdenes, brindándoles una experiencia más eficiente y accesible. Ahora pueden ver e interactuar con su pedido en tiempo real en un formato compacto y disponible en todo momento.


Press enter or click to view image in full size


## Artículos que te pueden interesar:


[Merge vs Rebase](https://engineering.rappi.com/merge-vs-rebase-208aa287f5f9)


[Memory Leaks — Parte 1/2](https://engineering.rappi.com/memory-leaks-parte-1-2-bf10d8c10b25)


[Memory Leaks — Parte 2/2](https://engineering.rappi.com/memory-leaks-parte-2-2-a8fc45aa35a8)


[Demo Apps en Rappi](https://engineering.rappi.com/demo-apps-en-rappi-f22a385792f7)


[Modular iOS Architecture at Rappi: Interface Modules](https://engineering.rappi.com/modular-ios-architecture-at-rappi-interface-modules-327b44f723e4)


[Improving iOS developer productivity and SwiftUI adoption in Rappi](https://medium.com/@pablocornejo/4a38d3661c53)


[Static Dispatch vs Dynamic Dispatch](https://medium.com/@frankgumeta/static-dispatch-vs-dynamic-dispatch-41fb0dc75821)


[iOS: Arquitecturas de aplicaciones](https://medium.com/p/37c3c7f4d094/edit)


## Squad de Tooling


El squad de Tooling Mobile se encarga de desarrollar y mantener las herramientas necesarias para la creación, pruebas y distribución de aplicaciones móviles en Rappi. Además, este squad es responsable de la infraestructura de CI/CD que garantiza la integridad de las aplicaciones y previene la introducción de código incorrecto.


Entre las herramientas que el squad de Tooling en Rappi crea para mejorar la productividad de los desarrolladores móviles se encuentran los plugins, la optimización del tamaño del binario, los tiempos de compilación locales, los analizadores de código estático, los linters, la administración de dependencias y muchas otras herramientas más. Este squad es fundamental para garantizar el éxito del desarrollo móvil en Rappi y para mantener la calidad y seguridad de la aplicación.


Dentro de sus responsabilidades podemos encontrar:


- Crear herramientas de productividad para los desarrolladores móviles
- Mejorar el tiempo de compilación local y en CI/CD
- Supervisar y disminuir el tamaño de la aplicación
- Mejorar y desarrollar herramientas de observabilidad


## Proyectos recientes


En los últimos meses, el squad de tooling ha estado trabajando en varios proyectos. Algunos de ellos incluyen:


### Team Scoring


El squad creó una iniciativa que busca mejorar la calidad del app de Rappi a través de la obtención de distintas métricas que nos permitan ver la evolución del estado de los equipos de forma resumida. Con esta herramienta es posible determinar no solo cuales son los equipos que requieren un mayor acompañamiento, sino que permite, además, identificar cuáles son las áreas que requieren mejora.


### Mobile Health Check


Es una herramienta que permite a los desarrolladores saber qué errores tienen al momento de querer compilar, un ejemplo podría ser, el tener la versión del IDE incorrecta. De esta forma los desarrolladores tendrán la autonomía para corregir o publicar el diagnóstico en algún canal de ayuda.


## Artículos que te pueden interesar:


[iOS Infra at Rappi](https://engineering.rappi.com/ios-infra-at-rappi-1598341069b8)


[Rappi’s iOS Bazel Build System Migration Journey](https://engineering.rappi.com/rappis-ios-bazel-build-system-migration-journey-eb1a2822a05)


[How Rappi deals with iOS App size](https://engineering.rappi.com/how-rappi-deals-with-ios-app-size-36dffec805d7)


[iOS app size reduction with Machine Outlining](https://engineering.rappi.com/ios-app-size-reduction-with-machine-outlining-5ef2c6b53237)


## Squad de Design System


El objetivo del squad es ayudar a los equipos a lograr una mayor eficiencia, coherencia y escalabilidad, así como realizar la implementación de nuevos componentes dentro del Design System.


Dentro de sus responsabilidades podemos encontrar:


- Crear un conjunto unificado y cohesivo de componentes visuales y pautas para todo el equipo móvil de Rappi
- Planificar estratégicamente el futuro del Design System con los diseñadores y el equipo de producto.
- Crear demos de alta calidad y prototipos interactivos para visualizar los componentes del Design System.
- Impulsar la adopción de nuestro Design System en toda la empresa, especialmente con el equipo de diseño y los desarrolladores móviles.


## Proyectos recientes


En los últimos meses, el squad de Design System ha estado trabajando en varios proyectos. Algunos de ellos incluyen:


### Adopción de SwiftUI y Jetpack Compose


SwiftUI y Jetpack Compose son los nuevos toolkit de UI para iOS y Android respectivamente y en Rappi nos encontramos en proceso de su adopción, para ello iniciamos utilizandolas para la creación de un nuevo Design System. Para esta integración realizamos un POC añadiendo estas tecnologías en la demo de Design System de cada plataforma.


### Creación de componentes de UI


En este proyecto nuestra tarea consiste en desarrollar una serie de nuevos componentes de UI para ponerlos a disposición de los equipos e incluirlos dentro de la demo, de modo que puedan ser utilizados en sus respectivas verticales.


Press enter or click to view image in full size


## Artículos que te pueden interesar:


[Building Design Systems at scale](https://engineering.rappi.com/building-design-systems-at-scale-323cb85d5047)


[Haptic en Rappi: Hacia una aplicación más accesible](https://engineering.rappi.com/haptic-en-rappi-hacia-una-aplicaci%C3%B3n-m%C3%A1s-accesible-153e9fe12b0)


[SwiftUI Previews in statically-linked iOS apps w/Cocoapods](https://medium.com/@omarlagunas/eb63fa1b0bc1)


[Android Atomic Design : Buscando aumentar la eficiencia del equipo de desarrollo.](https://medium.com/@misaelmce/android-atomic-design-buscando-aumentar-la-eficiencia-del-equipo-de-desarrollo-a1da8b62a890)


## Conclusión


La construcción del equipo de Core Mobile de Rappi ocurrió debido a una necesidad y se ha convertido en un elemento crucial para mantener una ventaja competitiva y estar al día con las tendencias del mercado. Este equipo ha buscado asegurar que las aplicaciones móviles sean técnicamente sólidas, escalables y seguras, y estén diseñadas de acuerdo con las mejores prácticas de la industria.


Además, al escuchar las preocupaciones y necesidades de los equipos de features y al estar alineadas las metas del equipo con las estrategias de la empresa, el equipo ayuda a mejorar rendimiento, fomentar la innovación y la creatividad de las demás verticales dentro de Rappi. En última instancia, esto ha ayudado a aumentar la satisfacción y lealtad de nuestros desarrolladores y ha brindado una experiencia de usuario más rápida, intuitiva y fácil de usar.
