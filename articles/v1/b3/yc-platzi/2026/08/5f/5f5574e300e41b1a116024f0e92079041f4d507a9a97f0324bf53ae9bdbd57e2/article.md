---
schema_version: "1.0.0"
document_id: "5f5574e300e41b1a116024f0e92079041f4d507a9a97f0324bf53ae9bdbd57e2"
company_key: "yc-platzi"
company: "Platzi"
source_id: "yc-platzi-news-import-15d3b242e96e"
canonical_url: "https://platzi.com/blog/ai-prompts-claude/"
published_at: null
first_seen_at: "2026-08-14T11:46:52.360271+00:00"
fetched_at: "2026-08-14T11:46:53.340923+00:00"
content_hash: "sha256:4c413df377b50e77203339d650ab92231fc0eed7e57485701bddc0142142edbf"
---

# Cómo hacer mejores prompts para Claude

Escribir mejores prompts en Claude se trata de **darle al modelo el contexto correcto, ordenar bien la información y decirle exactamente qué esperas** como resultado.


Veamos una estructura práctica basada en recomendaciones que dan los propios especialistas de Anthropic para crear prompts más claros, confiables y útiles.


## ¿Por qué importa escribir buenos prompts en Claude?


Claude, y cualquier modelo, puede resolver tareas complejas, pero sus respuestas dependen mucho de la claridad del pedido. Cuando una instrucción es ambigua, el modelo puede completar vacíos con suposiciones que parecen razonables, aunque no sean correctas.


Anthropic lo resume con una idea práctica: **Claude debe entender la tarea como la entendería una persona nueva en tu equipo. Si alguien con poco contexto no puede seguir tus instrucciones, probablemente Claude tampoco las interpretará bien.**


Por eso, un buen prompt no solo dice qué quieres. También explica el contexto, el objetivo, las restricciones, el formato esperado y el criterio para decidir cuándo una respuesta es suficiente.


## ¿Qué es prompt engineering?


El prompt engineering es la práctica de **diseñar instrucciones claras para que un modelo de lenguaje responda con más precisión, consistencia y utilidad.**


En Claude, esta práctica es especialmente importante cuando trabajas con tareas como análisis de documentos, extracción de información, revisión de código, investigación, automatización de procesos o generación de respuestas estructuradas.


Un buen prompt puede incluir:


- La tarea que debe resolver Claude
- El contexto que necesita para entenderla
- Los datos que debe analizar
- Las reglas que debe seguir
- Los ejemplos que muestran el resultado esperado
- El formato de salida
- Los criterios para verificar la respuesta


Antes de mejorar un prompt, Anthropic recomienda tener claro qué significa “éxito” para tu caso de uso y contar con una forma de probar si el resultado cumple esos criterios.


## La estructura de un buen prompt para Claude


No existe un prompt universal. La mejor estructura depende de la tarea. Aun así, una base útil para Claude puede organizarse así:


```text
<contexto>
explica el rol de Claude y el objetivo de la tarea.
</contexto>


<datos>
incluye la información que Claude debe analizar.
</datos>


<instrucciones>
describe los pasos que debe seguir.
</instrucciones>


<criterios>
define qué hace que una respuesta sea correcta o útil.
</criterios>


<ejemplos>
muestra casos similares y sus respuestas esperadas.
</ejemplos>


<formato_salida>
define cómo debe entregar la respuesta.
</formato_salida>


```


La clave no es hacer prompts largos. La clave es que cada parte tenga una función clara.


Anthropic recomienda **usar instrucciones explícitas, formatos definidos y pasos numerados** cuando el orden importa. También recomienda pedir el comportamiento esperado de forma positiva, es decir, explicar qué debe hacer Claude en lugar de solo decirle qué no debe hacer.


## 1. Empieza por el contexto


Un error común es pedir la respuesta final sin explicar el escenario.


Por ejemplo:


```text
Resume este documento.


```


Ese prompt puede funcionar para una tarea simple, pero deja muchas preguntas abiertas. ¿Para quién es el resumen? ¿Qué debe priorizar? ¿Debe mencionar riesgos? ¿Debe ser técnico, ejecutivo o educativo?


Un prompt más útil sería:


```text
Eres un asistente que ayuda a un equipo académico a revisar documentos técnicos. Resume el documento para una persona que necesita entender las ideas principales, los riesgos y las acciones recomendadas.


```


Ese contexto orienta el tipo de lectura que Claude debe hacer.


**Explicar el contexto o la motivación detrás de una instrucción** ayuda a que Claude entienda mejor el objetivo y entregue respuestas más ajustadas.


## 2. Define la tarea con precisión


Claude responde mejor cuando la instrucción es específica. No basta con decir “analiza”, “mejora” o “revisa”. Es mejor decir qué tipo de análisis necesitas.


No es lo mismo pedir:


```text
Analiza estos comentarios.


```


Que pedir:


```text
Clasifica estos comentarios de usuarios en problemas de producto, dudas de uso, quejas de precio y oportunidades de mejora. Después identifica los tres patrones más repetidos.


```


La segunda instrucción reduce la ambigüedad. También permite evaluar si Claude hizo bien la tarea.


Si quieres que Claude haga algo “extra”, como detectar riesgos, proponer alternativas o verificar contradicciones, debes pedirlo de forma explícita. Anthropic advierte que **no conviene depender de que el modelo infiera instrucciones que no escribiste.**


## 3. Separa información fija e información variable


Cuando usas Claude dentro de una aplicación, no todo cambia en cada consulta.


La **información fija** puede ser **el rol, las reglas, el tono, el formato de salida o el criterio de calidad** . La **información variable** puede ser **el texto del usuario, un documento, una transcripción, una imagen o datos** recuperados desde otro sistema.


Por ejemplo:


```text
<contexto_fijo>
Eres un asistente que clasifica tickets de soporte para una plataforma educativa.
</contexto_fijo>


<entrada_variable>
{{ticket_del_usuario}}
</entrada_variable>


```


Esta separación ayuda a mantener prompts consistentes, fáciles de probar y más simples de escalar.


Claude Console permite trabajar con plantillas y variables para distinguir contenido fijo y dinámico. Anthropic recomienda usar este enfoque cuando una parte del prompt se repetirá en varias llamadas a Claude.


## 4. Usa etiquetas XML para ordenar el prompt


Claude funciona bien cuando la información está delimitada. Una forma recomendada por Anthropic es **usar etiquetas XML para separar instrucciones, contexto, ejemplos y datos de entrada.**


Por ejemplo:


```text
<contexto>
Eres un asistente que revisa reportes de accidentes de tránsito.
</contexto>


<formulario>
Aquí va la información del formulario.
</formulario>


<boceto>
Aquí va la descripción o imagen del boceto.
</boceto>


<instrucciones>
Primero revisa el formulario. Luego interpreta el boceto usando el contexto del formulario. Finalmente, indica si hay suficiente evidencia para llegar a una conclusión.
</instrucciones>


```


Las etiquetas XML ayudan a Claude a distinguir qué parte del prompt es una instrucción, qué parte es información de entrada y qué parte es un ejemplo.


Anthropic recomienda usar nombres de etiquetas consistentes y descriptivos. También sugiere anidar etiquetas cuando hay jerarquía natural, como varios documentos dentro de una misma colección.


## 5. Dale un orden de análisis


En tareas complejas, el orden cambia el resultado.


Si Claude analiza primero una imagen ambigua y después lee el contexto, puede llegar a una conclusión débil. En cambio, si primero revisa el formulario, identifica los datos relevantes y luego interpreta la imagen, tendrá más información para responder.


**Un buen prompt no solo dice qué hacer. También dice en qué orden hacerlo.**


Por ejemplo:


```text
1. Identifica el objetivo del documento.
2. Extrae los datos relevantes.
3. Señala contradicciones o vacíos.
4. Evalúa si hay evidencia suficiente.
5. Entrega una conclusión con nivel de confianza.


```


Este tipo de instrucción es útil cuando necesitas consistencia, especialmente en flujos que se repiten, como análisis de documentos, clasificación de tickets, revisión legal o investigación.


## 6. Incluye ejemplos cuando la tarea sea ambigua


Los ejemplos son una de las formas más confiables de guiar a Claude.


Anthropic recomienda usar ejemplos cuando quieras controlar el formato, el tono o la estructura de la respuesta. También sugiere que los ejemplos sean relevantes, diversos y estructurados con etiquetas como` <example>` o` <examples>` .


Por ejemplo:


```text
<examples>
<example>
<input>
El usuario dice: “No encuentro dónde descargar mi certificado”.
</input>


<output>
Categoría: duda de uso
Prioridad: media
Respuesta sugerida: explicar dónde encontrar certificados en el perfil.
</output>
</example>
</examples>


```


**Los ejemplos no tienen que ser perfectos, pero sí deben enseñar el criterio que quieres que Claude aplique.**


Anthropic sugiere usar entre 3 y 5 ejemplos para mejorar la consistencia, sobre todo cuando hay casos límite o formatos muy específicos.


## 7. Define el formato de salida


Si la respuesta de Claude se usará en una interfaz, una base de datos o una automatización, el formato importa tanto como el contenido.


Puedes pedir una respuesta simple:


```text
Entrega un resumen breve y una recomendación final.


```


También puedes pedir una salida estructurada:


```text
<resumen>
explica brevemente qué ocurrió.
</resumen>


<riesgos>
lista los riesgos principales.
</riesgos>


<recomendacion>
indica el siguiente paso recomendado.
</recomendacion>


```


Para usos más técnicos, Claude también puede trabajar con salidas JSON estructuradas.


La documentación actual de Anthropic indica que **los structured outputs permiten obtener JSON válido que sigue un esquema definido** , y esto es útil para extracción de datos, reportes estructurados y respuestas de API.


Esto es importante porque, aunque un prompt puede pedir “responde en JSON”, una salida estructurada con esquema ofrece más confiabilidad cuando el resultado debe procesarse automáticamente.


## 8. Evita depender del prefill en modelos recientes


En versiones anteriores, una técnica común era iniciar la respuesta de Claude con una parte ya escrita. A eso se le conoce como prefill.


Por ejemplo, se podía iniciar la respuesta con:


```text
<veredicto_final>


```


Así Claude continuaba dentro de esa estructura y evitaba introducciones innecesarias.


Sin embargo,[la documentación actual de Anthropic sobre prompting](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) cambió esta recomendación. **Desde Claude 4.6 y Claude Mythos Preview, los prefills en el último turno del asistente ya no son compatibles en algunos casos** , y Anthropic recomienda migrar hacia instrucciones de formato, salidas estructuradas y mejores prompts.


Por eso, hoy conviene pensar el *prefill* como una técnica limitada, no como una práctica general para nuevos proyectos.


En aplicaciones nuevas, es más seguro definir el formato con claridad dentro del prompt o usar structured outputs cuando necesites JSON válido y fácil de procesar.


## 9. Usa adaptive thinking para tareas complejas


Claude puede usar capacidades de razonamiento para tareas que requieren varios pasos. Sin embargo, la forma recomendada de hacerlo depende del modelo.


La documentación actual de Anthropic recomienda adaptive thinking para Claude Opus 4.7, Claude Opus 4.6 y Claude Sonnet 4.6.


En Claude Opus 4.7, adaptive thinking es el único modo de thinking soportado y el modo manual con` budget_tokens` ya no se acepta.


Esto significa que **no conviene decir simplemente “usa extended thinking” como recomendación universal.**


Una mejor recomendación es: **usa thinking cuando la tarea lo justifique, especialmente en problemas de razonamiento complejo, análisis con herramientas, revisión de código o flujos agentic** . Para tareas simples, puede no ser necesario.


Anthropic también explica que[adaptive thinking permite que Claude decida cuándo y cuánto razonar según la complejidad de la solicitud.](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking) Además, puede combinarse con el parámetro` effort` para orientar la profundidad del razonamiento.


## 10. Ajusta el nivel de esfuerzo según la tarea


En los modelos recientes, Anthropic introduce el parámetro` effort` para equilibrar inteligencia, costo y latencia.


En términos prácticos: **no todas las tareas necesitan el mismo nivel de razonamiento** . Clasificar una frase simple puede requerir poco esfuerzo. Revisar código, analizar contratos o coordinar herramientas puede requerir más.


Anthropic recomienda usar niveles altos de esfuerzo en tareas sensibles a la calidad, como casos de programación, flujos agentic o análisis complejo. Para tareas cortas y de baja complejidad, un esfuerzo menor puede reducir costo y latencia.


El punto para es: mientras más compleja sea la tarea, más explícito debes ser sobre el nivel de análisis esperado.


## 11. Pide verificación antes de cerrar


Una buena instrucción **no solo pide una respuesta** . También puede **pedirle al modelo que revise si su respuesta cumple los criterios definidos** .


Por ejemplo:


```text
Antes de terminar, verifica que la respuesta:
1. use solo información incluida en el documento
2. no invente datos
3. incluya el nivel de confianza
4. siga el formato solicitado


```


Esto ayuda en tareas donde los errores son costosos, como análisis de documentos, datos, código o información técnica.


Anthropic recomienda pedir autoverificación contra criterios de prueba, especialmente en tareas de código y matemáticas. Esta idea también puede adaptarse a otros casos donde la precisión sea importante.


## Checklist para escribir mejores prompts en Claude


Antes de usar un prompt en una tarea importante, revisa esta lista:


✅ ¿Claude sabe cuál es su rol? ✅ ¿La tarea está descrita con claridad? ✅ ¿El contexto aparece antes de la instrucción final? ✅ ¿Los datos están separados de las instrucciones? ✅ ¿El modelo sabe qué hacer cuando falta información? ✅ ¿Hay un orden claro de análisis? ✅ ¿Incluiste ejemplos si la tarea es ambigua? ✅ ¿El formato de salida está definido? ✅ ¿Agregaste criterios para verificar la respuesta? ✅ ¿Probaste el prompt con casos fáciles y difíciles?


Si respondes “no” a varias preguntas, probablemente el problema no sea Claude. Probablemente el prompt todavía necesita más estructura.


## Ejemplo de prompt completo para Claude


Este ejemplo se puede adaptar a tareas de análisis documental:


```text
<contexto>
Eres un asistente especializado en analizar documentos operativos. Ayudas a un equipo a entender qué ocurrió, qué evidencia existe y qué decisión se puede tomar con la información disponible.
</contexto>


<criterios>
Debes ser factual y cuidadoso. No inventes detalles. Si los datos no permiten llegar a una conclusión confiable, dilo explícitamente.
</criterios>


<datos>
Recibirás un documento, notas del usuario y cualquier evidencia adicional disponible.
</datos>


<instrucciones>
1. Lee primero el documento principal.
2. Identifica los datos verificables.
3. Señala contradicciones, vacíos o información ambigua.
4. Evalúa si hay evidencia suficiente para responder.
5. Entrega una conclusión con nivel de confianza.
6. Verifica que tu respuesta no incluya datos inventados.
</instrucciones>


<formato_salida>
Devuelve la respuesta con esta estructura:


<resumen>
breve explicación del caso.
</resumen>


<evidencia>
lista de elementos observados.
</evidencia>


<limitaciones>
datos faltantes o ambiguos.
</limitaciones>


<conclusion>
respuesta final y nivel de confianza.
</conclusion>
</formato_salida>


```


> Este prompt no es una plantilla universal. Es una base que puedes ajustar según la tarea.


La mejora real ocurre cuando lo pruebas con casos reales, encuentras errores y ajustas el contexto, los ejemplos, el formato o los criterios de evaluación.


## El prompt engineering es un proceso vivo e iterativo


El primer prompt casi nunca es el mejor.


**Un prompt útil se construye probando, observando fallos y ajustando instrucciones** . Si Claude (o el modelo que estés usando) responde algo fuera de contexto, no basta con corregir la respuesta final. Hay que revisar qué parte del prompt permitió ese error.


Tal vez faltaba contexto. Tal vez el orden de análisis no estaba claro. Tal vez el formato de salida era ambiguo. Tal vez necesitabas ejemplos. Tal vez el modelo requería structured outputs en lugar de solo una instrucción de formato.


Ese es el cambio de mentalidad: **no escribir prompts como pedidos sueltos, sino como instrucciones diseñadas para trabajar con menos ambigüedad.**


Si quieres mejorar los resultados que obtienes al trabajar con IA, te recomendamos tomar el[Curso de Prompt Engineering](https://platzi.com/cursos/prompt-engineering/) de Platzi y aprender a estructurar instrucciones claras para trabajar mejor con modelos como Claude, ChatGPT, Gemini y Copilot.


Este post se creó con base en[la documentación](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) y las[recomendaciones de Anthropic](https://www.youtube.com/watch?v=ysPbXH0LpIE) , pero en Platzi sabemos que las mejores formas de trabajar con IA también aparecen cuando cada persona prueba, ajusta y adapta los prompts a sus propios retos.


👀 Por eso queremos leerte: **deja en los comentarios** ¿qué has probado al escribir prompts y qué te ha funcionado mejor? También cuéntanos si **has encontrado alguna técnica útil que no mencionamos en este artículo.**


Los mejores aprendizajes muchas veces aparecen en la práctica y una idea que para ti ya es parte de tu forma de trabajar puede ser justo lo que otra persona necesitaba para mejorar sus propios resultados con IA 💚
