---
schema_version: "1.0.0"
document_id: "a2feb52bc35a42a6e03167b6c5b04ab9ea55670fa89c781e0e39d2aa77576029"
company_key: "yc-henry"
company: "HENRY"
source_id: "yc-henry-rss-d34ee96ffff8"
canonical_url: "https://blog.soyhenry.com/como-integrar-la-api-de-openai-en-una-app-con-react-y-node-js/"
published_at: "2026-07-10T12:00:00+00:00"
first_seen_at: "2026-07-25T07:55:58.830099+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:4ce95af90450e047fe36a39fdce63d692478f9ff24334f55b74da151df14237a"
---

# Cómo integrar la API de OpenAI en una app con React y Node.js

Sumarle a una aplicación web una función inteligente —un asistente, un resumidor, un clasificador— dejó de ser terreno exclusivo de especialistas en IA. Con la API de OpenAI, cualquier desarrollador Full Stack puede hacerlo. El desafío real no es "llamar al modelo": es hacerlo con la arquitectura correcta, sin exponer tu clave y sin romper la experiencia de usuario. En esta nota te explicamos cómo integrar la API de OpenAI en una app con React y Node.js, paso a paso y con código listo para usar.


## **Qué significa integrar la API de OpenAI en una app web**


**Integrar la API de OpenAI en una app significa sumarle a tu producto una capa de inteligencia** : tu frontend captura lo que el usuario pide, tu backend lo envía al modelo a través de la API y devuelve la respuesta ya procesada a la interfaz. No entrenas ningún modelo ni corres nada pesado en tu servidor; solo orquestas una conversación entre tu aplicación y un modelo que ya existe.


Esto encaja de forma natural en el stack de un Full Stack moderno. React se ocupa de la interfaz, Node.js del backend y la API de OpenAI aporta el "cerebro". Saber conectar esas tres piezas es hoy una de las habilidades que más distingue a un[desarrollador Full Stack en 2026](https://blog.soyhenry.com/que-significa-ser-full-stack-developer-en-2026-desarrollo-asistido-por-ia/) , porque convierte una app común en un producto que entiende lenguaje natural.


*Si quieres construir este tipo de aplicaciones de punta a punta, la*[Carrera de Full Stack de Henry](https://www.soyhenry.com/webfullstack?utm_source=blog&utm_medium=organico&utm_campaign=comoIntegrarApiOpenAiReactNode) *enseña React, Node.js, TypeScript y la integración de APIs de IA con proyectos reales desde las primeras semanas.* 🚀


## **La regla de oro: la clave de API nunca va en el frontend**


Antes del primer paso, una decisión de arquitectura que separa una integración amateur de una profesional: **tu clave de API de OpenAI nunca debe estar en el código de React** . Todo lo que vive en el frontend es visible para cualquiera que abra las herramientas del navegador, así que una clave ahí equivale a regalarla, con el costo (y el riesgo) que eso implica.


La solución es simple y es el patrón estándar: **el frontend en React no habla directamente con OpenAI** , sino con tu propio backend en Node.js, que guarda la clave de forma segura y actúa como intermediario. El navegador llama a tu servidor, tu servidor llama a OpenAI, y la respuesta vuelve por el mismo camino. Con ese esquema en mente, vamos al paso a paso.


## **Cómo integrar la API de OpenAI con React y Node.js, paso a paso**


### **1️⃣ Prepara el proyecto y tu clave de OpenAI**


En la carpeta de tu backend, inicia el proyecto con` npm init -y` e instala en una sola línea las dependencias que vas a usar —Express como servidor, cors para permitir que React lo llame, dotenv para leer variables de entorno y la librería oficial de OpenAI— con` npm install express cors dotenv openai` .


Para obtener la clave, entra a tu cuenta en platform.openai.com, sección de API keys, crea una clave nueva y cópiala en ese momento, porque por seguridad se muestra una sola vez. Ten en cuenta que el uso de la API se cobra por consumo y es independiente de una suscripción a ChatGPT, así que la cuenta necesita saldo o un método de pago cargado.


Guarda esa clave en un archivo` .env` con el formato` OPENAI_API_KEY=tu_clave` , y agrega` .env` y` node_modules` a tu` .gitignore` para no subirlos nunca a GitHub.


### **2️⃣ Crea el backend con Node y Express**


El backend es un servidor de Express con un solo endpoint —por ejemplo` POST /api/chat` — que hace de intermediario. Al arrancar, importas Express, cors, dotenv y la librería de OpenAI, creas la app con` express()` y habilitas` cors()` y` express.json()` para poder recibir desde React peticiones en formato JSON.


Después instancias el cliente con` new OpenAI()` , que toma la clave desde` OPENAI_API_KEY` de forma automática. Dentro del endpoint tomas el` prompt` que llega en el cuerpo de la petición, se lo pasas al modelo con` client.responses.create({ model, input })` —usando la Responses API, la interfaz recomendada hoy— y devuelves el texto del resultado, que viene en` response.output_text` . Envuelve esa llamada en un` try/catch` para responder con un error claro si algo falla, y por último pones el servidor a escuchar en un puerto (por ejemplo, el 3001).


Un detalle: elige el modelo según tu caso. Para tareas simples o de alto volumen conviene una variante más liviana y económica (como GPT-5.4 mini); la lista actualizada está en la página de modelos de OpenAI.


### **3️⃣ Conecta el frontend en React**


En el componente de React manejas tres estados con` useState` : el texto que escribe el usuario, la respuesta del modelo y un booleano de "cargando". Cuando la persona envía el formulario, una función asíncrona hace un` fetch` a tu backend (no a OpenAI) con método` POST` , la cabecera` Content-Type: application/json` y el` prompt` en el cuerpo de la petición. Mientras espera, activa el estado de "cargando" para que la interfaz no parezca congelada; cuando llega la respuesta, la guarda en el estado y la muestra en pantalla; y si algo falla, muestra un mensaje de error. La clave del frontend es esa: siempre habla con tu servidor, nunca directamente con la API de OpenAI.


### **4️⃣ Prueba el flujo completo**


Levanta el backend (` node server.js` ) y la app de React, escribe algo en el campo de texto y envíalo. Si todo está conectado, verás la respuesta del modelo en pantalla en unos segundos. Si algo falla, revisa la consola del navegador y la del servidor: casi siempre el problema es la clave mal cargada o el backend apagado.


*Construir integraciones como esta, cada vez más completas, es exactamente lo que se practica en la*[Carrera de Full Stack de Henry](https://www.soyhenry.com/webfullstack?utm_source=blog&utm_medium=organico&utm_campaign=comoIntegrarApiOpenAiReactNode) *, con mentores que trabajan en la industria y proyectos que terminan en tu portfolio.* 💡


## **Buenas prácticas al llevar la integración a producción**


El flujo anterior funciona, pero un producto real necesita algunos cuidados extra:


- **Maneja la latencia de forma asíncrona:** una respuesta del modelo puede tardar varios segundos; el estado de "cargando" evita que la interfaz parezca congelada.
- **Controla el costo y el modelo:** cada llamada consume tokens; elegir un modelo más liviano para tareas simples reduce mucho el gasto.
- **Valida la entrada del usuario:** nunca confíes en el texto que llega del frontend; límpialo y ponle un límite de longitud antes de enviarlo al modelo.
- **Aprovecha los reintentos del SDK:** la librería de OpenAI reintenta sola los errores de red y de límite de uso, pero conviene registrar los fallos para detectarlos a tiempo.


📎Si quieres profundizar en estas decisiones —asincronía, reintentos y manejo de latencia—, esta guía sobre[cómo conectar una API de OpenAI en tu código](https://blog.soyhenry.com/de-modelos-a-aplicaciones-como-conectar-una-api-de-openai-en-tu-codigo/) lo desarrolla en detalle.


*Dominar estos detalles es lo que distingue a quien "usó la API una vez" de quien construye productos con IA de forma profesional. La*[Carrera de Full Stack de Henry](https://www.soyhenry.com/webfullstack?utm_source=blog&utm_medium=organico&utm_campaign=comoIntegrarApiOpenAiReactNode) *está diseñada para que llegues a ese nivel.* ⚡


## **En resumen**


- **Integrar la API de OpenAI** suma una capa de inteligencia a tu app: el frontend pide, el backend orquesta y el modelo responde.
- **La clave de API nunca va en el frontend:** React llama a tu backend en Node.js, que guarda la clave y actúa como intermediario seguro.
- El flujo tiene **cuatro pasos** : preparar el proyecto y la clave, crear el backend con Express, conectar el frontend en React y probar todo junto.
- **La Responses API** (client.responses.create) es la interfaz recomendada hoy, y conviene elegir el modelo según el costo y la tarea.
- **En producción** importan la latencia asíncrona, la validación de la entrada, el control de costos y el registro de errores.


## **Conclusión**


Integrar la API de OpenAI en una app con React y Node.js no requiere saber de machine learning: requiere entender cómo se comunican el frontend, el backend y el modelo, y respetar una regla de seguridad básica que muchos pasan por alto. Una vez que armas este flujo, el patrón se repite para casi cualquier funcionalidad inteligente que quieras sumar a un producto: un asistente, un buscador semántico, un generador de contenido. Esa capacidad de conectar las piezas es, hoy, uno de los mayores diferenciales de un perfil Full Stack.


*Si quieres pasar de integrar una API a construir productos completos con IA, la*[Carrera de Full Stack de Henry](https://www.soyhenry.com/webfullstack?utm_source=blog&utm_medium=organico&utm_campaign=comoIntegrarApiOpenAiReactNode) *te enseña el stack entero —React, Node.js, TypeScript, bases de datos y despliegue en la nube— con proyectos reales y un equipo de career coaches que te acompaña hasta conseguir empleo. Aplica y empieza a construir aplicaciones inteligentes de punta a punta.* 🚀


## **Preguntas frecuentes**


**¿Necesito saber de machine learning para integrar la API de OpenAI?**


No. Integrar la API es una tarea de desarrollo de software: enviar una petición, esperar la respuesta y procesarla. No entrenas ni ajustas modelos; usas uno que ya existe a través de la API. Con bases de JavaScript, React y Node.js es suficiente para empezar.


**¿Por qué no puedo llamar a la API de OpenAI directamente desde React?**


Porque tendrías que poner tu clave en el frontend, y todo lo que vive en el navegador es visible para el usuario. Cualquiera podría copiarla y usar tu cuenta. Por eso la llamada se hace desde el backend en Node.js, que guarda la clave de forma segura y actúa como intermediario.


**¿Qué modelo de OpenAI conviene usar?**


Depende del caso. Los modelos más potentes (como GPT-5.5) dan mejores respuestas en tareas complejas; las variantes más livianas (como GPT-5.4 mini) son más rápidas y económicas para tareas simples o de alto volumen. La lista vigente está en la página de modelos de OpenAI.


**¿Cuánto cuesta usar la API de OpenAI en mi app?**


La API se cobra por consumo, según la cantidad de texto que entra y sale del modelo, y es un costo aparte de cualquier suscripción a ChatGPT. Para controlar el gasto conviene elegir un modelo acorde a la tarea y limitar la longitud de las peticiones.


### Etiquetas


- [Desarrollo Web](https://blog.soyhenry.com/tag/desarrollo-web/)
- [Desarrollo Web Full Stack](https://blog.soyhenry.com/tag/desarrollo-web-full-stack/)
- [Henry](https://blog.soyhenry.com/tag/henry/)
- [Mundo Henry](https://blog.soyhenry.com/tag/mundo-henry/)
- [Trabajar en Tecnología](https://blog.soyhenry.com/tag/trabajar-en-tecnologia/)
