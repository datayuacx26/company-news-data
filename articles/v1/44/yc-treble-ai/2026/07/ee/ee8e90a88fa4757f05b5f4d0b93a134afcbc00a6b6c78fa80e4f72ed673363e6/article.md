---
schema_version: "1.0.0"
document_id: "ee8e90a88fa4757f05b5f4d0b93a134afcbc00a6b6c78fa80e4f72ed673363e6"
company_key: "yc-treble-ai"
company: "treble.ai"
source_id: "yc-treble-ai-rss-eeea5a3a95fb"
canonical_url: "https://treble.ai/blog/whatsapp-usernames-cambios-empresas-que-usan-la-api/"
published_at: "2026-07-02T15:38:39+00:00"
first_seen_at: "2026-07-20T23:20:57.574111+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:f73744d1f056e79c9cc2dabdc56bdde1a608e9ef8e80c756da6106160afd4eba"
---

# WhatsApp usernames y BSUID: qué cambia para empresas que usan la API

WhatsApp está introduciendo usernames y un nuevo identificador técnico, el BSUID. Entiende el impacto para operaciones con WhatsApp API y qué preparar antes.


---


WhatsApp anunció la llegada de los usernames, nombres de usuario únicos que permiten identificar y contactar personas sin depender del número de teléfono. Para los usuarios finales, es un cambio de experiencia. Para empresas que operan via WhatsApp Business API, el impacto es más técnico y requiere atención antes de que la funcionalidad esté activa.


## ¿ **Qué son los usernames en WhatsApp** ?


Un username es un identificador público elegido por el usuario (formato @nombredeusuario) que funciona en paralelo al número de teléfono. La persona puede ser encontrada y contactada via username, sin necesidad de compartir su número.


Desde la perspectiva del usuario, es un cambio de privacidad y conveniencia. Desde la perspectiva de la empresa, aparece una segunda forma de identificación del contacto que necesita ser considerada en toda la operación.


## ¿ **Qué es el BSUID** ?


Junto con los usernames, WhatsApp está introduciendo el BSUID (Business Solution User Identifier), un identificador técnico asignado desde el lado de la API que complementa al número de teléfono como forma de referenciar un contacto dentro de una operación de negocio.


En la práctica, esto significa que un contacto puede tener: un número de teléfono, un username y un BSUID. Dependiendo de cómo estén configurados los flujos y sistemas, estas tres referencias necesitan tratarse de forma consistente para no generar rupturas en conversaciones ni pérdida de contexto.


## **Qué puede verse afectado en tu operación**


*Identificación de contactos en los webhooks* Si tus webhooks usan el número de teléfono como clave primaria para identificar a un usuario, necesitas evaluar si la llegada del BSUID puede alterar el payload recibido y cómo responde el sistema actual a eso.


*Continuidad de conversaciones* Los flujos que dependen de una identificación precisa del contacto para mantener contexto — historial, etapa en el embudo, tickets abiertos — necesitan ser lo suficientemente robustos para tratar la nueva forma de identificación sin perder el hilo de la conversación.


*Datos en el CRM* Si almacenas contactos en el CRM con número de teléfono como identificador único, puede ser necesario preparar el modelo de datos para también almacenar el BSUID cuando esté disponible.


*Templates y opt-ins* La base de contactos para envío de campañas via WhatsApp API generalmente está construida alrededor del número de teléfono. El username no reemplaza el opt-in, pero es posible que nuevos flujos de captación empiecen a trabajar con usernames como punto de entrada.


## ¿ **Qué preparar ahora** ?


Aunque el cambio se está liberando de forma gradual, el momento de revisar la arquitectura es antes del impacto, no después. Vale la pena mapear:


Qué sistemas usan el número de teléfono como clave primaria para identificar contactos de WhatsApp; cómo están tratando hoy la identificación de usuario los webhooks; si la plataforma de WhatsApp API que utilizas ya tiene documentación sobre soporte al BSUID.


## ¿ **Cómo se está preparando Treble.ai** ?


Seguimos de cerca las actualizaciones de la API de WhatsApp para garantizar que las operaciones de nuestros clientes mantengan continuidad sin interrupciones. A medida que el soporte al BSUID se haga disponible oficialmente, comunicaremos qué cambia y qué hay que hacer del lado de la configuración.


## ¿Dudas sobre el impacto en tu operación? Escríbenos.


---


[Agenda una DEMO con Treble](https://treble.ai/landing-page-talk-to-sales)
