---
schema_version: "1.0.0"
document_id: "6690caa5d63b425bdf2313a7a3b9def5eb66b175d1dc7836921fdcaac73bd8a6"
company_key: "yc-fintoc"
company: "Fintoc"
source_id: "yc-fintoc-news-import-4ac2d54454f2"
canonical_url: "https://www.fintoc.com/cl/blog/presentando-el-mcp-de-fintoc"
published_at: null
first_seen_at: "2026-07-21T20:01:58.351478+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:e34f57096a3e20405df7955104129139ae999e36daddd1f0d1bb9cc468c8bf4c"
---

# Presentando el MCP de Fintoc

Actualmente equipos como Contabilidad, Finanzas y Operaciones usan información de Fintoc a diario y muchas veces necesitan combinarla con contexto interno (reportes específicos, casos puntuales). Con el MCP, pueden hacerlo directamente desde su agente de IA, sin tener que alternar entre el chat y el dashboard.


Hoy lanzamos[Fintoc MCP](https://docs.fintoc.com/docs/mcp) : un servidor MCP que conecta Fintoc directo al agente que ya usas. El agente consulta y opera recursos en base a tus permisos en nuestro dashboard.


## Tu Fintoc dentro del agente que ya usas


La mayoría del trabajo diario con Fintoc no es escribir código nuevo: es consultar el estado de un pago o reembolso y saldo pendiente por dispersar. Antes eso significaba abrir el dashboard o escribir un mini-script para los más técnicos. Ahora es una conversación:


```text
> ¿Cuánto me deberían dispersar mañana?
> ¿Se confirmó este reembolso? ¿Cuándo se ejecutó?
```


## Documentación en contexto


El tool` search_documentation` le da al agente acceso a la documentación pública de Fintoc en tiempo real. Cuando un desarrollador pide ayuda para una integración nueva, el agente consulta la documentación relevante antes de escribir la primera línea, sin pestañas extras ni copy-paste.


```text
> ¿Cómo integro el checkout de Fintoc en mi web?
```


Resultado: el agente lee la doc vigente, propone código alineado a la API actual y reduce los errores de integración que pueden resultar de una lectura directa o transcripción.


## Operación conversacional


Cuando algo falla en producción, el flujo típico hoy es: copiar el id del recurso, abrir el dashboard, buscar manualmente, tratar de entender. Con el MCP, puedes consultar cualquier recurso de la API por ID directo desde el chat e incluso filtrar por estados:


```text
> ¿Qué pasó con pi_3a8...? Lístame también los payment intents
> que quedaron en failed en las últimas 24h


> Cada cuánto se concilian los pagos?


> Cómo hago un reembolso?
```


El agente devuelve el detalle del recurso y un resumen de los fallidos. Útil para operaciones y para equipos de soporte interno que hoy dependen del equipo de ingeniería para diagnosticar.


## Permisos a tu medida


Para usar el MCP, se te pedirá ingresar en el dashboard de Fintoc, autenticarte y autorizar a Claude, por ejemplo, a acceder a la información a la que tú tienes acceso en Fintoc. Tú eliges los permisos que tendrá el agente. Si no quieres que pueda hacer reembolsos, basta con que selecciones que el agente no tenga esos permisos al autenticarte.


## Úsalo donde ya trabajas


Cualquier cliente MCP compatible con OAuth 2.1 puede conectarse a` mcp.fintoc.com` sin contacto previo con Fintoc. Ya soportamos consultas con:


- Claude code
- Codex
- Cursor


Y cualquier cliente nuevo que aparezca en el ecosistema queda habilitado automáticamente.


## Pruébalo hoy


Usar el MCP viene incluido con todos los productos Fintoc sin costo adicional.


Si usas ChatGPT,[instala el conector acá](https://chatgpt.com/apps/fintoc/asdk_app_6a188ecb09708191a9f2222212ba715b) .


Si usas otro agente, agrega el servidor` mcp.fintoc.com` . El cliente abre el browser, te lleva a confirmar permisos de acceso de Fintoc, eliges modo (test o live), y vuelves al chat listo para empezar a consultar.


Si quieres ver la documentación paso a paso,[entra acá](https://docs.fintoc.com/docs/model-context-protocol-mcp) .


Si tienes feedback sobre el producto o algo que te gustaría que agreguemos, escríbenos!product@fintoc.com .


‍
