---
schema_version: "1.0.0"
document_id: "f0c8605479bf46674ef5f292ee547b8c421e23f217a015e272ce049ada538387"
company_key: "yc-henry"
company: "HENRY"
source_id: "yc-henry-rss-d34ee96ffff8"
canonical_url: "https://blog.soyhenry.com/como-crear-tu-primer-agente-de-ia-con-langgraph-guia-paso-a-paso/"
published_at: "2026-07-13T22:18:00+00:00"
first_seen_at: "2026-07-25T07:55:58.830099+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:d928da513e7f8630e15c90d8fc8b9bea4d68fcc07f20c4058d5dfe32a5288164"
---

# Cómo crear tu primer agente de IA con LangGraph: guía paso a paso

Un chatbot responde una pregunta y se detiene. Un agente de IA, en cambio, puede razonar, decidir usar una herramienta, ver el resultado y seguir hasta resolver una tarea completa. Ese salto —de responder a actuar— es una de las habilidades más buscadas hoy en AI Engineering, y LangGraph es uno de los frameworks que lo hace posible. En esta nota te explicamos, paso a paso, **cómo crear tu primer agente de IA con LangGraph en Python** , desde qué es un agente hasta cómo ejecutarlo.


## **Qué es un agente de IA y en qué se diferencia de un chatbot**


Un agente de IA es un sistema que usa un modelo de lenguaje como "cerebro" para razonar, decidir qué herramienta usar y ejecutar acciones en varios pasos, hasta cumplir un objetivo, en lugar de limitarse a responder una sola vez. La diferencia con un chatbot común está justamente ahí: el chatbot genera una respuesta y termina; **el agente entra en un ciclo de "pensar, actuar y volver a pensar" hasta que la tarea está lista** .


Ese patrón tiene nombre: se lo conoce como **ReAct** (Reasoning and Acting). El modelo razona sobre qué hacer, llama a una herramienta —buscar en la web, consultar una base de datos, hacer un cálculo—, lee el resultado y decide el siguiente paso. Repite ese bucle las veces que haga falta, y recién cuando no necesita más herramientas entrega la respuesta final.


📎 Si quieres una base más general sobre qué es un agente, sus componentes (modelo, memoria y herramientas) y cómo se piensa desde cero, esta guía sobre[cómo crear un agente de IA desde cero](https://blog.soyhenry.com/como-crear-un-agente-de-ia-desde-cero-guia-definitiva-para-2026/) lo desarrolla en detalle.


*Construir estos sistemas es el corazón del trabajo de un AI Engineer. La*[Carrera de AI Engineering de Henry](https://www.soyhenry.com/carrera-ai-engineering?utm_source=blog&utm_medium=organico&utm_campaign=comoCrearPrimerAgenteLangGraph) *te enseña a diseñar agentes, sistemas RAG y aplicaciones de IA con proyectos reales y mentores de la industria.* 🚀


## **Qué es LangGraph y por qué se usa para construir agentes**


**LangGraph es un framework open source** , creado por el equipo de LangChain, para construir agentes modelándolos como un grafo. En lugar de escribir una secuencia lineal, defines un **estado** compartido (la información que viaja entre pasos, normalmente el historial de mensajes), unos **nodos** que hacen el trabajo y unas **aristas** que deciden el flujo, incluidos los ciclos en los que el agente piensa, actúa y vuelve a pensar. Esa **capacidad de manejar bucles y ramificaciones** es lo que lo hace ideal para agentes, algo que las cadenas lineales no resuelven bien.


**LangGraph ofrece dos niveles de trabajo, y conviene conocerlos desde el principio.** El camino rápido es usar una función prebuilt que arma el agente por ti; el camino manual es construir el grafo nodo por nodo, cuando necesitas control fino sobre la lógica. La recomendación práctica para tu primer agente es empezar por el camino rápido y bajar al manual solo cuando el proyecto lo pida. Más adelante, al armar el agente, veremos en la práctica cómo se ve cada camino.


*Si quieres dominar este tipo de herramientas con acompañamiento profesional, la*[Carrera de AI Engineering de Henry](https://www.soyhenry.com/carrera-ai-engineering?utm_source=blog&utm_medium=organico&utm_campaign=comoCrearPrimerAgenteLangGraph) *está diseñada para que aprendas el stack real del rol —Python, LangChain, LangGraph y APIs de modelos— construyendo proyectos.* 💡


## **Cómo crear tu primer agente de IA con LangGraph, paso a paso**


### **1️⃣ Prepara el entorno y tu clave de modelo**


**Instala las librerías necesarias en tu entorno de Python** : el paquete langgraph y el conector del modelo que vayas a usar, por ejemplo langchain-openai (con pip install langgraph langchain-openai). Necesitas también una **clave de API** del proveedor del modelo —OpenAI, Anthropic o el que prefieras—, que se carga como variable de entorno (por ejemplo, OPENAI_API_KEY) para no escribirla en el código.


**Sobre el costo:** recorrer un tutorial como este suele costar menos de dos dólares en llamadas a la API, y cada consulta con un par de herramientas ronda unos centavos. Si prefieres evitar el gasto en la nube, LangGraph también funciona con modelos locales a través de Ollama, aunque para eso conviene tener una máquina con buena memoria.


### **2️⃣ Define las herramientas que podrá usar el agente**


Las herramientas (tools) son, simplemente, funciones de Python que el agente puede invocar. Se marcan con el decorador @tool de langchain_core.tools, y el detalle clave es el **docstring** : esa descripción es lo que lee el modelo para decidir cuándo usar cada herramienta, así que tiene que ser clara. Una función que busca el clima, que consulta una base de datos o que hace un cálculo son ejemplos típicos de tu primer agente.


### **3️⃣ Arma el agente**


El camino rápido es la función *create_react_agent* del módulo *langgraph.prebuilt* : le pasas el modelo, la lista de herramientas y un prompt de sistema, y ella construye por dentro todo el ciclo de razonamiento y acción. Con esas tres líneas conceptuales ya tienes un agente funcional.


Por debajo, ese atajo arma un **StateGraph** : un estado con la lista de mensajes, un nodo que llama al modelo, un nodo que ejecuta las herramientas (ToolNode) y una arista condicional que, según si el modelo pidió o no una herramienta, vuelve al modelo o termina. **Compilar ese grafo (compile()) te da el agente** . Empezar por *create_react_agent* te evita ese detalle hasta que necesites personalizarlo. Un apunte de actualidad: el ecosistema de LangChain está unificando este atajo en un helper más nuevo llamado *create_agent* ; si te cruzas ese nombre, es la evolución de la misma idea.


### **4️⃣ Ejecútalo y observa su razonamiento**


Para correr el agente, le pasas un mensaje del usuario con *.invoke({"messages": \[...\]})* . Vas a ver el bucle en acción: el modelo razona, decide llamar a una herramienta, recibe el resultado y decide el siguiente paso hasta responder. Para entender qué "piensa" en cada paso y depurar cuando algo falla, conviene conectar LangSmith, la herramienta de observabilidad del mismo ecosistema.


*Armar agentes como este, cada vez más complejos, es parte del día a día de un AI Engineer. En la*[Carrera de AI Engineering de Henry](https://www.soyhenry.com/carrera-ai-engineering?utm_source=blog&utm_medium=organico&utm_campaign=comoCrearPrimerAgenteLangGraph) *los construyes sobre proyectos reales, con feedback de mentores que trabajan en la industria.* ⚡


## **Qué hace realmente potente a un agente (y qué cuidar)**


Un primer agente funciona con lo anterior, pero pasar de la demo a algo confiable exige atender algunos puntos:


- **Docstrings claros en las herramientas:** el agente decide qué herramienta usar leyendo su descripción, así que una descripción vaga produce decisiones malas.
- **Memoria entre pasos:** para conversaciones de varios turnos, se agrega checkpointing, que guarda el estado del grafo y le permite recordar el contexto.
- **Supervisión humana en acciones sensibles:** LangGraph permite pausar el flujo para que una persona apruebe antes de ejecutar algo delicado (human-in-the-loop).
- **Costo y latencia:** cada paso del ciclo es una llamada al modelo; un agente que itera varias veces cuesta y tarda más, así que conviene elegir bien el modelo y acotar la cantidad de pasos.


*Dominar estas decisiones es lo que separa a quien probó un agente una vez de quien construye sistemas que funcionan en producción. La*[Carrera de AI Engineering de Henry](https://www.soyhenry.com/carrera-ai-engineering?utm_source=blog&utm_medium=organico&utm_campaign=comoCrearPrimerAgenteLangGraph) *está pensada para llegar a ese nivel.* 🚀


## **En resumen**


- Un **agente de IA** usa un modelo como cerebro para razonar, usar herramientas y actuar en varios pasos, a diferencia de un chatbot que responde una sola vez.
- **LangGraph** modela el agente como un grafo con estado, nodos y aristas, lo que le permite manejar los ciclos de "pensar y actuar" propios de un agente.
- El camino rápido es **create_react_agent** ; por debajo arma un StateGraph con un nodo de razonamiento, un nodo de herramientas y una arista condicional.
- Los **cuatro pasos** son: preparar el entorno, definir las herramientas, armar el agente y ejecutarlo observando su razonamiento.
- En un agente real importan **la memoria, la supervisión humana, el costo por iteración y la calidad de las descripciones** de cada herramienta.


## **Conclusión**


Crear tu primer agente de IA con LangGraph no exige inventar nada desde cero: exige entender el ciclo de razonar y actuar, definir buenas herramientas y dejar que el framework orqueste el flujo. Una vez que ese primer agente funciona, el mismo patrón escala hacia sistemas mucho más ambiciosos: agentes con memoria, con varias herramientas, con supervisión humana y desplegados en producción. Esa capacidad de pasar de la idea a un sistema que decide y actúa es, hoy, uno de los diferenciales más valorados en un perfil de AI Engineering.


*Si quieres pasar de tu primer agente a construir sistemas de IA completos, la*[Carrera de AI Engineering de Henry](https://www.soyhenry.com/carrera-ai-engineering?utm_source=blog&utm_medium=organico&utm_campaign=comoCrearPrimerAgenteLangGraph) *te forma con proyectos reales de agentes, RAG y aplicaciones de IA, con mentores de la industria y un equipo de career coaches que te acompaña hasta conseguir empleo. Aplica y empieza a construir agentes que resuelven tareas de verdad.* 🚀


## **Preguntas frecuentes**


**¿Necesito saber mucho de machine learning para crear un agente con LangGraph?**


No. Crear un agente es, sobre todo, programación en Python y lógica de orquestación: defines herramientas, conectas un modelo y armas el flujo. No entrenas modelos; usas uno que ya existe a través de su API. Con bases sólidas de Python es suficiente para empezar.


**¿Cuál es la diferencia entre LangChain y LangGraph?**


LangChain aporta los bloques de construcción (conexión a modelos, herramientas, prompts), mientras que LangGraph se ocupa de la orquestación: cómo esos bloques se conectan, forman ciclos y mantienen un estado. Para un agente que necesita razonar en varios pasos, LangGraph es la pieza que maneja ese flujo.


**¿Conviene usar create_react_agent o construir el grafo a mano?**


Para tu primer agente y para la mayoría de los casos estándar, create_react_agent (o el nuevo create_agent de LangChain) es el camino recomendado, porque arma el ciclo por ti. Construir el StateGraph a mano tiene sentido cuando necesitas control fino: ejecución en paralelo, ramificaciones complejas o lógica de reintento personalizada.


**¿Cuánto cuesta ejecutar un agente de IA?**


Depende del modelo y de cuántos pasos dé el agente, ya que cada paso es una llamada a la API. Un agente simple de práctica cuesta apenas centavos por consulta. Para reducir el gasto conviene elegir un modelo acorde a la tarea, limitar la cantidad de iteraciones o usar un modelo local con herramientas como Ollama.


### Etiquetas


- [Cómo hacer](https://blog.soyhenry.com/tag/como-hacer/)
- [Henry](https://blog.soyhenry.com/tag/henry/)
- [Mundo Henry](https://blog.soyhenry.com/tag/mundo-henry/)
- [Trabajar en Tecnología](https://blog.soyhenry.com/tag/trabajar-en-tecnologia/)
