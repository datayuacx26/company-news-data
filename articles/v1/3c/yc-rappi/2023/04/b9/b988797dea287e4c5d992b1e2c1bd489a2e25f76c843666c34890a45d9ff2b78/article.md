---
schema_version: "1.0.0"
document_id: "b988797dea287e4c5d992b1e2c1bd489a2e25f76c843666c34890a45d9ff2b78"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/android-atomic-design-buscando-aumentar-la-eficiencia-del-equipo-de-desarrollo-a1da8b62a890"
published_at: "2023-04-19T15:25:33+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:c2648e20dc405ed621147c5ca8599ad62fa5b331a50b2622d45f0f94e90d3209"
---

# Android Atomic Design : Buscando aumentar la eficiencia del equipo de desarrollo.

# Android Atomic Design : Buscando aumentar la eficiencia del equipo de desarrollo.


[Misael E. Muñoz C](https://medium.com/@misaelmce?source=post_page---byline--a1da8b62a890---------------------------------------)


7 min read


·


Mar 28, 2023


--


Press enter or click to view image in full size


La creación de interfaces de usuario coherentes y escalables es uno de los principales desafíos en el desarrollo de aplicaciones móviles. Para enfrentar este desafío, los diseñadores y desarrolladores han adoptado diferentes metodologías a lo largo del tiempo. Una de estas metodologías es Atomic Design, una técnica de diseño que se centra en la creación de componentes de diseño reutilizables y modulares para construir interfaces de usuario escalables y coherentes.


En este artículo profundizaremos en cómo se puede aplicar Atomic Design en un proyecto Android y los beneficios que esta metodología puede aportar. Exploraremos cómo los componentes de diseño, como *átomos, moléculas, organismos y plantillas,* pueden utilizarse para crear una interfaz de usuario coherente y escalable. También veremos cómo la implementación de Atomic Design puede mejorar la eficiencia del equipo de desarrollo y la experiencia del usuario en general, Así como tambien, la restructuración del uso de átomos dentro de la implementación en Rappi.


En definitiva, si estás interesado en mejorar el proceso de diseño y desarrollo de aplicaciones móviles para Android, y te gustaría conocer más sobre Atomic Design y sus beneficios, este artículo es para ti.


## ¿Qué es Atomic Design?


Atomic Design es una metodología de diseño que se centra en la creación de componentes de diseño reutilizables y modulares para construir interfaces de usuario escalables y coherentes. Esta metodología, desarrollada por el diseñador Brad Frost, se basa en el concepto de que una interfaz de usuario puede descomponerse en componentes más pequeños y elementales, como átomos, moléculas, organismos y plantillas.


Los átomos son los componentes más simples e indivisibles de una interfaz de usuario, como los colores, los iconos, los tipos de letra y los botones. Las moléculas son la combinación de varios átomos para formar elementos más complejos, como los formularios, los encabezados y los menús. Los organismos son combinaciones más grandes de moléculas y átomos, como los encabezados de página, los pies de página y los bloques de contenido. Finalmente, las plantillas son la estructura y el diseño general de la interfaz de usuario, que se construyen a partir de los organismos.


Muy bien, esa es la definición de Atomic Design por Brad Frost, pero si analizamos un poco Frost describe los átomos como elementos que dejan de ser funcionales si se descomponen aún más. Él clasifica los botones y las etiquetas de formas como átomos por esta razón, pero aquí es donde comienza la confusión. Los elementos más complejos, como Formularios, también pueden dejar de ser funcionales si empezamos a desmantelarlos y clasificarlos como un organismo. Esto deja mucho espacio para la interpretación de lo que es un átomo o no.


A pesar de que el modelo se actualizó para incluir tokens de diseño, esta no sigue la metáfora de la química.


Press enter or click to view image in full size


Una de las soluciones que acoplamos en Rappi, es que solo los estilos fundamentales son átomos porque realmente no se pueden descomponer más. Cualquier cosa en la categoría de átomos se puede convertir en fichas de diseño. Con este enfoque, los átomos solo pueden crear una molécula cuando se combinan con otro átomo. Entonces, una etiqueta de formulario, al ser una combinación de un átomo de color y un átomo de texto, se consideraría una molécula.


Cuando los estilos fundamentales son átomos, se asegura que de un proyecto a otro los tipos de átomos sean muy parecidos. Con este enfoque, los átomos que hemos identificado son, *colores, dimensiones, tipografias, iconos, estilos, y formas.*


Con este cambio a los átomos que solo incluyan los estilos fundamentales de un sistema de diseño, las otras categorías de este enfoque deben actualizarse para reflejar el cambio. Como es el caso de las moléculas ya que solo serán elementos que se pueden descomponer en átomos, aunque en ocasiones pueden contener otras moléculas, como un botón que contiene un icono.


*Experimentemos con algunos ejemplos plasmado dentro de nuestro design system.*


## Atomic Design: identificando los átomos


Como lo mencioné anteriormente, Los átomos son la mínima expresión de las interfaces, tienen un gran impacto pues definen las propiedades que tendrán todos los elementos y componentes dentro del sistema de diseño, el cual podemos descomponerlo en los siguientes átomos:


Simplemente miramos los átomos como la mínima expresión de componente de diseño.


Una vez que somos capaces de identificar los átomos, podemos obtener una mejor comprensión de cómo crear una molécula.


## Atomic Design: identificando las moléculas


Las moléculas son componentes simples dentro de la interfaz y están conformados por átomos.


## Get Misael E. Muñoz C’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


También pueden contener otras moléculas simples dentro de ellos (es decir, un botón puede incluir un icono dentro de él).


Press enter or click to view image in full size


Como se pueden observar en los ejemplos de arriba, tenemos los botones que es un ejemplo claro de como se puede componer una molecula de átomos simples como tipografia, formas, colores y finalmente de una molécula de texto que sirve como contador. De la misma manera, los TextFields que estan compuestos de átomos de tipografia, dimensiones, colores, iconos, y de una molécula de Text que hace parte del componente final del componente de nuestro design system.


## Atomic Design: identificando los organismos


Los organismos son componentes complejos dentro de la interfaz y están conformados por átomos, y moléculas.


Necesitamos agrupar las moléculas para construir un organismo con mayor funcionalidad que sea capaz de trabajar por sí mismo.


Press enter or click to view image in full size


En el ejemplo se puede observar dos organismos que son muy comunes en sistemas de diseño de android, como lo son los TopBars, y BottomBars, los cuales si se ven detalladamente, podemos notar que están compuestos por átomos, y moléculas, como lo es el caso del TopBar, que se compone de átomos de Text, Iconos y finalmente por una molécula de IconButton, por otro lado el BottomBar, se compone de una molécula común que contiene átomos de Iconos, Tipografias, Color que a su vez componen la molécula de opción del BottomBar.


## Atomic Design: identificando las Plantillas


Las plantillas son herramientas útiles en el diseño de interfaces de usuario, ya que permiten a los diseñadores crear diseños y patrones reutilizables para aplicaciones. Al utilizar plantillas, se pueden establecer de antemano los elementos de diseño necesarios para construir una interfaz de usuario coherente y escalable.


Una plantilla es, de hecho, un conjunto de organismos que, cuando se unen, conducen a la formación de un esquema (layout) para las páginas.


Press enter or click to view image in full size


En el ejemplo vemos dos templates base de nuestro design system en Rappi, donde son templates que estan creados con un Scaffold como base y utiliza los organismos de TopBar o BottomBar, de esta manera nuestro equipo de desarrollo puede partir de esta plantilla para crear pantallas con caracteristicas diferentes pero con los templates y componentes base de nuestro design system.


## Beneficios de Atomic Design


La implementación de Atomic Design en nuestro proyecto nos aportar diversos beneficios, entre los cuales se destacan:


1. Mejora la coherencia visual: Al construir una interfaz de usuario utilizando componentes reutilizables y modulares, se puede lograr una coherencia visual entre las distintas partes de la aplicación. Esto significa que los usuarios pueden navegar por la aplicación con mayor facilidad, ya que los elementos visuales son consistentes en toda la aplicación.
2. Aumenta la eficiencia del equipo de desarrollo: Al tener componentes de diseño reutilizables, el equipo de desarrollo puede ahorrar tiempo y esfuerzo en el proceso de diseño y desarrollo. Esto significa que el equipo de desarrollo puede concentrarse en la creación de nuevas funcionalidades y mejoras en lugar de reinventar la rueda cada vez que se necesita un nuevo componente de diseño.
3. Facilita la escalabilidad de la aplicación: Al construir una interfaz de usuario utilizando componentes modulares y reutilizables, se puede escalar la aplicación con mayor facilidad. Esto significa que se puede agregar nuevos elementos a la interfaz de usuario sin tener que reconstruir la aplicación desde cero, lo que ahorra tiempo y esfuerzo en el proceso de desarrollo.


## **En conclusión**


La implementación de Atomic Design en un proyecto de desarrollo de aplicaciones para Android puede aportar una serie de beneficios significativos, como mejorar la coherencia visual, aumentar la eficiencia del equipo de desarrollo, facilitar la escalabilidad de la aplicación y promover la consistencia de la marca. Al identificar los átomos y moléculas en la interfaz de usuario, se pueden construir componentes más complejos y reutilizables, lo que ahorra tiempo y esfuerzo en el proceso de desarrollo.


En general, Atomic Design es una metodología de diseño sólida que puede ayudar a los equipos de desarrollo a crear aplicaciones Android más eficientes y coherentes visualmente.


*Agradecimientos a*


[Romel Palomino](https://medium.com/u/b112dc3f6e88?source=post_page---user_mention--a1da8b62a890---------------------------------------)
