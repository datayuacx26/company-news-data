---
schema_version: "1.0.0"
document_id: "ba9d4008bccf337735542f6ba47fc42b3800eb42bacc51d8400e51c02ba8c87e"
company_key: "yc-henry"
company: "HENRY"
source_id: "yc-henry-rss-d34ee96ffff8"
canonical_url: "https://blog.soyhenry.com/como-escribir-buenos-prompts-guia-de-prompt-engineering-con-ejemplos-3/"
published_at: "2026-07-22T11:50:33+00:00"
first_seen_at: "2026-07-25T07:55:58.830099+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:e9bb3154a1f88dfc2ca781d47789e42a9af1116a3b752c1a88a8d9b3c54362b4"
---

# Cómo escribir buenos prompts: guía de prompt engineering con ejemplos

Asignar un rol, dar contexto, especificar detalladamente la tarea, indicar formato de salida; son condiciones necesarias para un prompt, pero no suficientes. Lo que de verdad marca la diferencia es más sutil: qué técnicas funcionan según la tarea, por qué los modelos de razonamiento se promptean distinto que los clásicos, y cómo cambia el prompt ideal según la herramienta que uses. En esta nota te explicamos todo esto, con ejemplos, para que **aprendas a construir prompts efectivos** .


## **Qué es (y qué dejó de ser) el prompt engineering en 2026**


El prompt engineering es la práctica de estructurar bien lo que le pides a un modelo de IA —el contexto, la tarea y el formato esperado— para obtener una respuesta precisa y útil. **La definición no cambió, pero sí lo que funciona** . Durante años, el juego consistía en acumular instrucciones, ejemplos y roles para llevar al modelo de la mano. Hoy, con modelos que infieren la intención mucho mejor, ese exceso de andamiaje a veces sobra e incluso empeora el resultado.


Ahora debes enfocarte en:


- Darle al modelo el **contexto correcto** (la información y los ejemplos que necesita para no inventar)
- **Elegir la técnica adecuada** para el modelo y la tarea.


A esta segunda capa —gestionar qué información entra y cómo— se la empieza a llamar **context engineering, y es la evolución natural del prompting** .


Escribir buenos prompts, entonces, es menos un arte de frases ingeniosas y más una habilidad de especificar con precisión.


*Saber trabajar con IA con criterio ya es parte del estándar en cualquier perfil tech. Si quieres convertirlo en una habilidad profesional, conoce y aplica a*[las carreras de Henry](https://www.soyhenry.com/carreras-ia?utm_source=blog&utm_medium=organico&utm_campaign=comoEscribirBuenosPromptsGuia) *, donde la IA atraviesa toda la formación.* 🚀


## **Las técnicas que de verdad cambian el resultado**


Veamos cuáles son las técnicas con mayor impacto real y cuándo conviene usar cada una.


#### **Escribe el prompt como una especificación, no como un pedido.**


Rol, contexto, tarea y formato es la base, pero el detalle es lo que decide: un rol decorativo («eres un experto en marketing») aporta poco; lo que mueve la aguja es **definir para quién es el resultado** , qué decisión va a alimentar y bajo qué restricciones.


Compara los resultados de estos prompts:


- Genérico: «escribe un email de ventas»
- Específico: «escribe un email de ventas de 80 palabras para un gerente de finanzas escéptico, que abra con un dato concreto y cierre con una sola llamada a la acción».


En este último, cada palabra acota el espacio de respuestas posibles.


#### **Define el criterio de éxito, incluido lo que no quieres.**


El modelo no adivina tu estándar de calidad. Además de indicar el formato de salida (una tabla, JSON, una longitud exacta), conviene **explicitar las restricciones negativas** —«sin tecnicismos», «sin inventar cifras»— y darle una salida para la incertidumbre: «si los datos no alcanzan, dilo en lugar de completar». Ese permiso para decir "no sé" es una de las formas más simples de **reducir respuestas inventadas** .


#### **Ancla la respuesta en material de referencia y limita al modelo a usarlo.**


Pegar el documento o los datos dentro del prompt no alcanza; el salto de calidad viene de instruir *«responde únicamente con la información de arriba y cita la parte que uses»* . Un detalle técnico que casi nadie aprovecha: en contextos largos, los modelos prestan más atención al principio y al final que al medio, así que la instrucción clave y lo más importante conviene ubicarlos en los extremos, no en medio.


#### **Usa ejemplos (few-shot), pero elígelos por cobertura, no por perfección.**


Dos o tres ejemplos de lo que esperas mejoran mucho la consistencia en los modelos clásicos. El hallazgo contraintuitivo de la investigación es que importa más que los ejemplos cubran la variedad de casos —incluidos los difíciles y las excepciones— que el hecho de que cada etiqueta sea impecable. Un aviso: **demasiados ejemplos hacen que el modelo copie patrones superficiales** , y en los modelos de razonamiento suelen estorbar más que ayudar.


#### **Encadena y hazlo iterar sobre su propia respuesta.**


Para tareas complejas, **dividir en prompts encadenados** —donde la salida de uno alimenta al siguiente— supera a un único prompt gigante.


Y una técnica de alto retorno: pedirle al modelo que revise su propia respuesta contra los criterios que le diste y la corrija («revisa el borrador anterior, verifica que cumpla cada condición y reescríbelo si falla alguna»). Ese **paso de autocrítica** levanta la calidad sin que tengas que rehacer el prompt desde cero.


## **El cambio más importante: modelos de razonamiento vs modelos clásicos**


Este es el punto que más resultados arruina hoy, y el que casi nadie ajusta bien. Conviven dos tipos de modelos, y cada uno pide un estilo de prompt casi opuesto.


Los **modelos de razonamiento** (la familia "o" de OpenAI, Claude con pensamiento extendido, Gemini en modo Deep Think, DeepSeek R1) ya razonan internamente antes de responder. Pedirles de forma explícita que "razonen paso a paso" no desbloquea nada: en el mejor caso desperdicia el presupuesto de razonamiento que ya tenían asignado. Con ellos conviene dar un objetivo claro y bien delimitado, ser conciso, evitar el few-shot que los encorseta y, cuando la herramienta lo permite, indicar cuánto esfuerzo de razonamiento aplicar. La regla práctica: empieza simple y agrega detalle solo si la respuesta se queda corta.


Los **modelos clásicos o de chat** (los de respuesta rápida) son lo contrario: se benefician del andamiaje. Ahí sí ayudan pedir el razonamiento explícito, los ejemplos few-shot y una estructura detallada. La regla acá es la inversa: empieza con estructura y ejemplos, y recorta si sobra.


Elegir mal el modelo también es un error: usar uno de razonamiento para redactar un mail rápido es lento y excesivo, y usar uno de chat para un problema lógico difícil baja la calidad.


*📎 Para ver cómo esta habilidad se traduce en empleabilidad real y por qué el mercado ya la da por sentada, puedes leer esta nota sobre[cómo trabajar con inteligencia artificial y cómo te prepara Henry para el mercado laboral](https://blog.soyhenry.com/como-trabajar-con-inteligencia-artificial-asi-te-prepara-henry-para-el-mercado-laboral/) .*


## **Cómo cambia el prompt según la herramienta**


El mismo prompt no rinde igual en todos los modelos. Cada uno tiene su "dialecto", y conocerlo hará que puedas escribir prompts efectivos.


### **Claude (Anthropic)**


Responde especialmente bien a **instrucciones explícitas y a la estructura con etiquetas tipo XML** (por ejemplo, envolver los ejemplos o el contexto en etiquetas propias), que le ayudan a separar cada parte del prompt. Para tareas difíciles, su modo de pensamiento adaptativo decide cuánto razonar; conviene dejarlo trabajar en lugar de sobre-instruirlo.


### **ChatGPT (OpenAI)**


Los modelos más nuevos funcionan como un sistema que enruta la petición al modelo adecuado según la dificultad. Por eso conviene un **tono conversacional y directo** , probar primero sin ejemplos (infiere muy bien la intención desde poco contexto) y no recargar de indicaciones de razonamiento las tareas complejas, porque puede perjudicar el resultado. En apps de producción, fijar una versión concreta del modelo evita sorpresas cuando cambia el comportamiento.


### **Gemini (Google)**


Prefiere prompts más cortos y directos que Claude o ChatGPT. Su gran fortaleza es el contexto enorme que admite: puedes darle documentos largos completos, pero justamente por eso **importa dónde ubicas la instrucción clave** dentro de todo ese material. Además, brilla cuando el prompt combina texto con imágenes u otros formatos.


### **GitHub Copilot**


Acá el "prompt" es, en buena parte, tu propio código. Copilot se guía por los archivos abiertos, los nombres de tus funciones y los comentarios: un comentario claro que describe la intención («función que valida un email y devuelve verdadero o falso») produce mejores sugerencias que un nombre vago. Para preguntas sobre todo el proyecto, su chat permite referirse al repositorio completo, y las instrucciones personalizadas fijan tu estilo de código.


### **DeepSeek**


Su modelo de razonamiento (R1) es open source y muy fuerte en lógica y matemática a bajo costo. Se prompt-ea como cualquier modelo de razonamiento: prompts simples y directos, sin ejemplos ni indicaciones de razonamiento de más. Al plantear el problema con claridad y dejarlo razonar, entrega lo mejor de sí.


*Dominar estas diferencias es lo que convierte la IA en una herramienta de trabajo real. Da el paso: aplica a*[las carreras de Henry](https://www.soyhenry.com/carreras-ia?utm_source=blog&utm_medium=organico&utm_campaign=comoEscribirBuenosPromptsGuia) *y aprende a usarla en proyectos concretos.* 💡


## **Errores que cometen incluso los usuarios avanzados**


- **Usar el mismo prompt en todas las herramientas:** una instrucción que funciona en ChatGPT puede rendir distinto en Claude o Gemini; conviene probar antes de estandarizar.
- **Sobre-instruir a un modelo que ya razona:** recargarlo de indicaciones de razonamiento suele restar, no sumar.
- **No dar material de referencia:** sin contexto real, el modelo llena los huecos inventando, con confianza.
- **Copiar prompts sin entenderlos:** un prompt "viral" rara vez encaja con tu caso; el valor está en saber por qué funciona y adaptarlo.
- **Elegir el modelo equivocado para la tarea:** potencia de razonamiento para algo trivial, o un modelo rápido para un problema complejo, desperdician tiempo o calidad.


*Aplica a*[Henry](https://www.soyhenry.com/carreras-ia?utm_source=blog&utm_medium=organico&utm_campaign=comoEscribirBuenosPromptsGuia) *y conviértete en el experto en IA que las empresas necesitan.* ⚡


## **En resumen**


- Un buen prompt es una especificación: define la tarea, el contexto, el formato de salida y también lo que hay que evitar, no solo un "rol".
- Anclar la respuesta en material de referencia e indicarle al modelo que use solo eso es la forma más efectiva de frenar las respuestas inventadas.
- Los modelos de razonamiento piden lo opuesto que los clásicos: con los primeros, menos instrucciones y más objetivo; con los segundos, ejemplos y estructura.
- Cada herramienta tiene su dialecto: etiquetas en Claude, tono directo en ChatGPT, prompts cortos con mucho contexto en Gemini, código y comentarios en Copilot, prompts mínimos en DeepSeek.
- El mayor salto de nivel no es saber más técnicas, sino elegir la técnica y el modelo correctos para cada tarea.


## **Conclusión**


Escribir buenos prompts dejó de ser un truco y se convirtió en una habilidad con capas: entender qué técnica pide cada tarea, saber que los modelos de razonamiento juegan con reglas distintas y conocer el dialecto de cada herramienta. Esa capacidad de **trabajar con IA con criterio** , y no de repetir fórmulas, es hoy una de las habilidades más valoradas en cualquier perfil tecnológico.


*Si quieres desarrollar esta habilidad, aplica a*[Henry](https://www.soyhenry.com/carreras-ia?utm_source=blog&utm_medium=organico&utm_campaign=comoEscribirBuenosPromptsGuia) *. Nuestras carreras integran el trabajo con IA, con mentores de la industria y acompañamiento hasta que consigas empleo.* 🚀


## **Preguntas frecuentes**


**¿El prompt engineering sigue sirviendo si los modelos son cada vez mejores?**


Sí, aunque cambió. Los modelos infieren mejor la intención, así que el andamiaje elaborado importa menos, pero las instrucciones claras y el buen contexto importan más que nunca. La habilidad ya no es memorizar trucos, sino elegir la técnica correcta para cada modelo y tarea.


**¿Conviene pedirle a los modelos nuevos que "razonen paso a paso"?**


En los modelos de razonamiento, no: ya razonan internamente antes de responder, así que pedírselo no agrega nada y puede desperdiciar el esfuerzo que ya tenían reservado. Esa técnica sigue siendo útil, en cambio, en los modelos clásicos de respuesta rápida.


**¿Un mismo prompt funciona igual en Claude, ChatGPT y Gemini?**


No necesariamente. Comparten principios, pero cada uno tiene preferencias: Claude responde bien a etiquetas y a instrucciones explícitas, Gemini prefiere prompts más cortos, y ChatGPT infiere bien desde poco contexto. Conviene probar el mismo pedido en la herramienta que uses antes de darlo por definitivo.


**¿Necesito saber programar para aprender prompt engineering?**


No. Es una habilidad transversal que sirve tanto para escribir y analizar como para programar o automatizar procesos. Escribir buenos prompts se apoya en pensar con claridad y estructurar bien lo que pides, más que en escribir código.


### Etiquetas


- [Cómo hacer](https://blog.soyhenry.com/tag/como-hacer/)
- [Desarrollo Web](https://blog.soyhenry.com/tag/desarrollo-web/)
- [Desarrollo Web Full Stack](https://blog.soyhenry.com/tag/desarrollo-web-full-stack/)
- [Henry](https://blog.soyhenry.com/tag/henry/)
- [Trabajar en Tecnología](https://blog.soyhenry.com/tag/trabajar-en-tecnologia/)
