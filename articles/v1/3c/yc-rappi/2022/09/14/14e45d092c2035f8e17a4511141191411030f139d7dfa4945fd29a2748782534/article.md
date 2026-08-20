---
schema_version: "1.0.0"
document_id: "14e45d092c2035f8e17a4511141191411030f139d7dfa4945fd29a2748782534"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/memory-leaks-parte-2-2-a8fc45aa35a8"
published_at: "2022-09-01T21:44:29+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:2148b2891c84bec479838afe3ccc52e0b74a7c0eea0705f244a4c62874781822"
---

# Memory Leaks — Parte 2/2

# [Memory Leaks — Parte 2/2](https://engineering.rappi.com/memory-leaks-parte-1-2-bf10d8c10b25)


[FrankGumeta](https://medium.com/@frankgumeta?source=post_page---byline--a8fc45aa35a8---------------------------------------)


7 min read


·


Sep 1, 2022


--


Press enter or click to view image in full size


[Anıl Karakaya](https://www.pexels.com/es-es/foto/manos-reparar-arreglando-trabajo-manual-6419128/)


Varias semanas han pasado desde que expliqué,[cómo funciona el manejo de memoria en iOS y su evolución a lo largo del tiempo](https://engineering.rappi.com/memory-leaks-parte-1-2-bf10d8c10b25) , vimos también que son los **retain cycles** y los **memory leaks** . Damas y caballeros, sin mucho más preámbulo, pasenle a lo barrido, coloquense su gorrito de hechicero porque hoy aprenderemos algunos trucos de magia para detectar, encontrar y corregir esos memory leaks que hasta ahora se nos han escapado como duendes en la oscuridad de la noche.


## ¿Con que los detectamos?


Hay dos maneras principales de detectar **memory leaks** . La primera es utilizar **Instruments** , que es la herramienta de profiling por excelencia y está disponible desde **Xcode 3** , la segunda es utilizar la herramienta de **Memory Graph** incluida dentro de **Xcode desde su version 8.**


## El problema


Creemos un memory leak en un proyecto de ejemplo. Analicemos las siguientes piezas de código.


```text
class   Hijo {       var   padre: Padre?      init  (padre: Padre? =  nil  ) {           self  .padre = padre          print(“Objeto \( Self  . self  ) inicializado”)      }      deinit   {          print(“Objeto \( Self  . self  ) liberado”)      }  }  class   Padre {      var   hijo: Hijo?      init  (hijo: Hijo? =  nil  ) {          self  .hijo = hijo          print(“Objeto \( Self  . self  ) inicializado”)      }      deinit   {          print(“Objeto \( Self  . self  ) liberado”)      }  }
```


Podemos ver la clase **Padre** que tiene una propiedad.
**Hijo** : Referencia **fuerte** a un objeto de tipo **Hijo**


Por su parte la clase **Hijo**
**Padre** : Referencia **fuerte** a un objeto de tipo **Padre**


Creemos pues una vista sencilla que contenga un botón y ejecute el siguiente fragmento de código.


Press enter or click to view image in full size


Vista ultra compleja de ejemplo


```text
@IBAction    func   memoryLeak() {      var   hijo: Hijo? = Hijo() // 1       var   padre: Padre? = Padre(hijo: hijo) // 2      hijo?.padre = padre // 3      hijo =  nil   // 4        padre =  nil   // 5  }
```


Analicemos la causa de nuestro Memory Leak.


1.-) Inicializamos un objeto tipo **Hijo** 2.-) Inicializamos un objeto tipo **Padre** pasando la instancia **Hijo** como parámetro.
3.-) Al objeto **Hijo** le asignamos el objeto que creamos en el paso 2
4.-) Intentamos forzar la liberación del objeto ***hijo***
5.-) Intentamos forzar la liberación del objeto ***padre***


Si nosotros corremos nuestra aplicación y presionamos el botón que creamos y que ejecuta nuestro método **memoryLeak()** veremos lo siguiente en la consola.


```text
Objeto Hijo inicializado  Objeto Padre inicializado
```


Pero como podrán notar los objetos no se liberan ya que no vemos los mensajes que indican que esto ha ocurrido. Estamos pues ante un Memory Leak.


¡Ahora si, manos a la obra!


## Método 1 — Instruments


Vayamos a **Xcode** y busquemos en **Menú > Product > Profile** (o podemos simplemente presionar **CMD + I** ), esto hará que nuestro proyecto compile y al finalizar deberíamos ver lo siguiente en pantalla.


Press enter or click to view image in full size


Aquí escogeremos la opción que dice Leaks y presionarémos el boton **choose** *(azul abajo a la derecha)*


Esto nos abrirá una segunda ventana que es donde haremos nuestro proceso de búsqueda e identificación.


Presionaremos el botón de grabación (Esquina superior izquierda) y apenas la aplicación se lance en nuestro simulador presionaremos el botón que creamos y que dispara la lógica que acabamos de analizar


Press enter or click to view image in full size


Lo primero que vas a notar al presionar el botón es que la línea de tiempo se va llenando con una gráfica que representa la cantidad de memoria que nuestra app va consumiendo en la sección de Allocations


Press enter or click to view image in full size


Pero vamos por partes, ¿qué es lo que estamos viendo aquí?


1. Aqui vemos el indicador de Leaks, si ves un **icono verde** , significa que en ese periodo de tiempo **no hubo leaks** , caso contrario, si vemos el **icono rojo** , significa que ella no te ama y que **Instruments** **detectó leaks** en ese periodo de tiempo.
2. **Al seleccionar** con el ratón el periodo de tiempo que tiene el icono rojo del punto anterior podremos ver una lista con todos los objetos con problemas.
3. **Al seleccionar** alguno de los objetos del punto 2, podrás ver el **stack trace** asociado a ese objeto.


Ya de entrada en el stack trace podemos ver la **clase** *(ViewController)* y el **método** *(memoryLeaks),* lo cual nos dice dónde está el problema.


Pero veamos más a profundidad


## Get FrankGumeta’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Si seleccionamos alguno de los pasos marcados en rojo del stack trace veremos qué **Instruments** nos lleva a la clase, método y linea donde esta el objeto que conforma el retain cycle.


Press enter or click to view image in full size


Si vamos al área de detalle y escogemos **Cycles & Roots** ( **CMD + 2** ) podremos ver como se conforma el retain cycle a detalle


Press enter or click to view image in full size


Por tanto, con esto encontramos la clase, el método y el objeto que causa el memory leak.


## Metodo 2 — Memory Graph Debugger


Memory Graph Debugger es una herramienta integrada en **XCode** que simplifica el proceso de visualización en tiempo real del contenido y estructura de los objetos que conforman el uso de memoria de nuestra app


Para debuggear el uso de memoria en búsqueda de leaks solo tenemos un requisito que cumplir.


En **Menú > Product > Scheme > Edit Scheme** y deberás activar la opción **Malloc Stack Logging** para poder ver los stack traces de los objetos en memoria


Press enter or click to view image in full size


Una vez esto hecho, corremos nuestra app como de costumbre y presionaremos el botón que creamos para generar los leaks.


Press enter or click to view image in full size


Una vez mas, vamos por partes


1. Primero presionaremos el botón del **Memory Graph Debugger** para ver los objetos que tenemos en memoria.
2. **Filtraremos** la lista de objetos en memoria para mostrar únicamente aquellos que son Memory Leaks.
3. **Seleccionaremos** alguno de los objetos que en la lista para ver su causa, al centro podrás ver una representación visual de los objetos relacionados al issue que seleccionaste.
4. Una vez más podremos ver el **stack trace** del objeto y si damos click en el botón de flecha podremos ir a la línea que causa el problema.


Press enter or click to view image in full size


Con esto, una vez mas, llegamos a la clase, método y objeto que causa el memory leak que estamos buscando


## Oye pero… ¿Y la solución?


Press enter or click to view image in full size


Si llegaste hasta este punto del artículo sin entender qué causó el memory leak regresa a[la primera parte](https://engineering.rappi.com/memory-leaks-parte-1-2-bf10d8c10b25) de esta serie donde explico porqué se generan.


De cualquier manera, te explico las posibles soluciones pero primero repasemos el problema.


Si volvemos a ver el código que se ejecuta al presionar el botón


```text
@IBAction    func   memoryLeak() {  var   hijo: Hijo? = Hijo()        var   padre: Padre? = Padre(hijo: hijo)        hijo?.padre = padre //El problema está aquí       hijo = nil          padre = nil     }
```


El problema radica en que padre e hijo tienen una relación mutua fuerte, evitando que ARC pueda determinar cuándo liberar el objeto que creamos.


Por lo tanto, hay 3 soluciones posibles en este caso, el cual dependerá de lo que en la práctica queramos hacer.


1. Eliminar la línea para evitar la relación mutua
2. En la clase **Padre** definir la variable hijo como **weak**
3. En la clase **Hijo** definir la variable padre como **weak**


Una vez hecho esto, podremos por fin ver como nuestros objetos se liberan correctamente


Press enter or click to view image in full size


## Conclusiones


En este ejemplo sencillo aprendimos como utilizar **Instruments** y **Memory Graph Debugger** , para **encontrar** y **ubicar** dónde ocurren los Memory Leaks. Una vez que encuentras la clase y lógica responsable solo queda **analizar** los objetos involucrados y encontrar **la mejor solución para tu caso concreto** .


Recuerda que la solución siempre será distinta para cada caso, pero siempre radica en analizar las relaciones fuertes y romper el retain cycle que causa el memory leak.


Hay otros trucos que puedes usar para detectar **retain cycles** como usar **Symbolic Breakpoints** o incluso usar **Unit Tests** para detectar cuando los objetos no se liberan lo cual significa **un potencial** memory leak **.**


Te invito a que me cuentes qué te pareció esta miniserie, si te gustaria aprender mas a profundidad cómo y para qué usar **Instruments** , cuentame también que **** otros temas te gustaria leer. Todos los comentarios son bienvenidos. Por lo pronto es todo.


¡Hasta la proxima!


**Bibliografia**


[Gathering information about memory use](https://developer.apple.com/documentation/xcode/gathering-information-about-memory-use)
[Detect and diagnose memory issues](https://developer.apple.com/videos/play/wwdc2021/10180/)
[View debugger and memory graph](https://www.youtube.com/watch?v=I25PQC-sBB8)
