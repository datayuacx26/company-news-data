---
schema_version: "1.0.0"
document_id: "b0f40093afb175190cda6a1a9c65ecbb69459ff0782b20ac64dc0cc7d345503d"
company_key: "yc-ruuf"
company: "RUUF"
source_id: "yc-ruuf-news-import-7ad24376001b"
canonical_url: "https://ruuf.cl/blog/la-tecnologia-de-ruuf-optimiza-cada-proyecto-solar"
published_at: "2025-10-21T18:59:40.114+00:00"
first_seen_at: "2026-07-23T23:37:01.985486+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:41762b6a7ec012576fe137bcfd33568c4a2c675d4217820d2a4b6e7a4fb87f23"
---

# La tecnología de RUUF optimiza cada proyecto solar

### Una pastelería solar


Imagina que RUUF es una pastelería. Cada día abre sus puertas sin saber qué van a pedir los clientes:


-


A veces es una sola torta de chocolate para 20 personas.


-


Otras veces son cinco tortas distintas: de frambuesa, de limón, sin azúcar, y cada una para un número diferente de invitados.


-


Y en los días más intensos, llegan pedidos de diez tortas diferentes: algunas de dos pisos, otras con decoraciones frágiles, otras veganas, y todas deben estar listas al mismo tiempo.


El problema es que nadie sabe con certeza qué se pedirá mañana. Y no basta con tener harina y azúcar: hay que calcular con precisión materiales, cantidades y el orden de producción para poder cumplir con todas las tortas.


En RUUF ocurre lo mismo. Cada proyecto solar es único, porque cada techo, comuna y familia tiene sus propias condiciones. Instalar energía solar no es simplemente “poner paneles solares en un techo”, requiere cálculos exactos de inclinación, cableado, materiales y proyección de consumo a futuro.


### La pregunta clave: ¿y la tecnología?


En las últimas semanas he conversado con devs de distintos lados y hay una pregunta que se repite: **¿dónde está la tecnología en RUUF?** Más de una vez me han dicho: *“ustedes tienen un equipo tecnológico muy bueno, pero no entiendo por qué”* .


La gran mayoría de la tecnología en RUUF no está a la vista. Más del 90% de nuestro código vive en el *backend* , y por lo mismo, pasa desapercibido *. Es como la cocina de la pastelería y no el mesón que todos ven* . Y por lo mismo, suele pasar desapercibido.


Pero en RUUF no programamos por programar. Nos hacemos preguntas concretas:


-


¿Qué podríamos estar haciendo mejor?


-


¿Qué podemos automatizar?


-


¿Dónde está hoy el cuello de botella?


Un ejemplo muy claro es el cálculo de materiales que se necesita para cada instalación. Si hoy lo hiciéramos manualmente, necesitaríamos contratar al menos dos personas adicionales dedicadas exclusivamente a esa tarea. Personalmente, no me gustaría ser una de esas personas.


### El nacimiento de Vlad


Acá entra algo clave de nuestra cultura: la colaboración constante entre desarrollo y operaciones. El equipo de operaciones (¡gracias, Pipe! 🙌) definió los *inputs* (las posibles variables que influyen en qué se necesitará), transformó el problema en cálculos comprensibles y nos ayudó a entenderlo desde el terreno.


De ahí en adelante, el desafío fue llevarlo a un sistema: estandarizable, modular y extensible, porque los requisitos y materiales cambian con el tiempo. Implementamos fórmulas de caída de tensión, cálculos de longitud de cableado según tamaño de tableros, selección de fijaciones según tipo de cubierta… Y sí, también nos equivocamos: una vez incluso enviamos a un cliente **25 veces más rieles de aluminio de lo necesario** .


Lograr llegar a una solución general que funcione para todo tipo de instalación requirió mucho esfuerzo. Fue un proceso largo, de mucho cuestionamiento, toma de decisiones y sufrimiento. Es un sistema muy complejo, donde las fórmulas dependen unas de otras; un error en un cálculo puede afectar los resultados de otros materiales, y comprobar si el resultado es correcto no siempre es fácil.


Por esa misma dificultad, tuvimos que desarrollar nuestras propias herramientas que nos faciliten el desarrollo y la comunicación. Herramientas que nos permiten comparar nuestros resultados con los cálculos realizados por el equipo de operaciones, obtener una visión del progreso y tener más claridad sobre qué tipo de instalaciones o materiales requieren mayor desarrollo. Fue un proceso de mucha iteración y colaboración. Partimos desarrollando que funcione para una instalación simple de cinco paneles, luego diez paneles, luego con zanjas, hasta finalmente llegar a instalaciones de clientes reales. Con nuestras pruebas alcanzamos a cubrir un total de 36 casos distintos.


De todo ese proceso nació **Vlad** . Con un set de *inputs* estándar (tipo de techo, distancias, cadenas solares ( *strings* ), protecciones, comuna, etc.), Vlad devuelve en milisegundos las cantidades necesarias de materiales para un proyecto. Calcula desde el diámetro de las tuberías hasta si es necesario arrendar andamios.


Ejemplo de resultados que retorna Vlad


### Lo que Vlad habilita


Tener a Vlad como base nos ha permitido avanzar mucho más allá del cálculo de materiales. Hoy podemos:


-


**Optimizar costos** : si las tejas son delicadas, el sistema lo considera automáticamente y sugiere los refuerzos adecuados.


-


**Proyectar gastos desde el inicio** : incluso cuando alguien completa el formulario en[ruuf.cl](https://ruuf.cl/) y obtiene un presupuesto, por detrás ya se ejecutan estos cálculos para estimar gastos reales.


-


**Controlar facturas** : como los datos son nuestros, podemos validar que lo facturado por proveedores corresponda a lo que efectivamente se compró.


### Lo que viene


Cada mejora tecnológica significa que podemos llegar a más casas, más rápido, y con mejores resultados. Pero sabemos que todavía queda mucho camino por recorrer.


Cosas como integrar cargadores de auto eléctrico, expandirnos a la realidad de otros países, identificar más tipos de falla y gestionar el arreglo automáticamente. Vlad es solo uno de los muchos ejemplos de cómo en RUUF usamos la tecnología para mejorar procesos, estandarizar instalaciones y asegurar calidad.


Si te interesa participar en un equipo que combina desarrollo tecnológico con impacto real en la **descarbonización del planeta** , revisa nuestras vacantes abiertas en[ruuf.cl/jobs](https://ruuf.cl/trabaja-en-ruuf) . Todavía tenemos mucho por hacer, muchas más familias a las que llegar y mucha energía limpia por instalar.
