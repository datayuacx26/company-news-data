---
schema_version: "1.0.0"
document_id: "7241b5ed974849aeda4ba65eeabb5eaec6621652dfb3f736a2ba27ef72a8106a"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/como-asociar-prefijos-en-la-dian-guia-paso-a-paso"
published_at: "2026-07-24T02:32:00+00:00"
first_seen_at: "2026-07-26T05:04:08.979462+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:d720bfb05c363b1b349e51ef3a5713e84e6ac29debe44021d30690ee016b3a0b"
---

# Cómo asociar prefijos en la DIAN: guía paso a paso

**Asociar prefijos en la DIAN** es el proceso mediante el cual vinculas los rangos de numeración de facturación electrónica —previamente autorizados por la entidad— a tu proveedor tecnológico o software de facturación. Sin este paso, tu sistema no podrá emitir facturas electrónicas válidas, aunque ya tengas la resolución de numeración aprobada. Se hace directamente en el portal de la DIAN, requiere un token de acceso enviado al correo registrado en tu RUT, y toma entre 10 y 30 minutos si tienes todo listo.


Este trámite suele generar dudas porque cada software (Siigo, Alegra, Finppi, Xubio, entre otros) documenta el proceso desde su propia plataforma, mezclando pasos de la DIAN con pasos internos de cada sistema. Aquí encontrarás la guía completa, neutral y ordenada: qué necesitas antes de empezar, cómo hacerlo en el portal de la DIAN, cómo confirmarlo en tu software contable y qué hacer si algo falla.


## ¿Qué significa "asociar prefijos" y por qué es obligatorio?


Cuando la DIAN te autoriza una resolución de numeración de facturación electrónica, esa autorización queda registrada a tu nombre, pero **no está vinculada automáticamente a ningún software** . Asociar el prefijo es el paso que le indica a la DIAN: "quiero facturar con este consecutivo, usando este proveedor tecnológico". Solo después de esta vinculación tu software puede transmitir facturas válidas con esa numeración.


Si omites este paso, o lo haces con el proveedor equivocado, la DIAN rechazará la validación de tus facturas o el prefijo simplemente no aparecerá disponible en tu sistema contable.


## Requisitos previos antes de asociar tus prefijos


Antes de entrar al portal de la DIAN, verifica que tengas:


- **Resolución de numeración electrónica aprobada** y en estado definitivo (no provisional).
- **RUT actualizado** , con correo electrónico vigente y correctamente registrado (ahí llegará el token de acceso).
- **Cédula y datos del representante legal** , si te registras como empresa (persona jurídica).
- **Nombre exacto de tu proveedor tecnológico o software de facturación** (por ejemplo, el que te haya indicado tu proveedor: Siigo, Alegra, Cifrato, Finppi, etc.), ya que deberás seleccionarlo dentro del portal.
- Al menos **2-3 horas de margen** después de que la resolución quede en estado definitivo, antes de intentar la asociación, para evitar que el sistema de la DIAN no reconozca la resolución.


## Guía paso a paso para asociar prefijos en la DIAN


1. **Ingresa al portal de la DIAN** ([www.dian.gov.co](http://www.dian.gov.co/) ) y busca, en la sección "Temas de interés", la opción **Factura electrónica → Facturando Electrónicamente** .
2. **Selecciona tu tipo de usuario** : "Empresa" (persona jurídica, ingresando cédula del representante legal y NIT) o "Persona" (persona natural, con tu tipo y número de documento).
3. **Autentícate con tus datos** y confirma. El sistema te informará que el enlace de acceso llegará al correo electrónico registrado en tu RUT.
4. **Revisa tu correo y haz clic en el enlace o token de acceso recibido.** Este token suele tener una validez de 30 a 60 minutos; si vence, deberás solicitar uno nuevo.
5. **Dentro del portal, ve al menú "Configuración" y selecciona "Rangos de numeración".**
6. **En el campo "Proveedor - Software", elige el nombre exacto de tu proveedor tecnológico** (el que efectivamente vas a usar para transmitir tus facturas).
7. **En el campo "Prefijo", selecciona la resolución que deseas asociar** y haz clic en **"Agregar"** .
8. **Confirma la acción en la ventana emergente** , seleccionando **"Aceptar"** .
9. **Repite el proceso** si tienes varias resoluciones que asociar (por ejemplo, factura de venta, factura POS o contingencia).
10. **Verifica el listado de resoluciones asociadas** en la parte inferior de la pantalla, junto con su fecha de asociación, para confirmar que el proceso quedó registrado correctamente.


## Después de asociar: sincroniza con tu software contable


Asociar el prefijo en la DIAN es solo la mitad del proceso. Para que tu sistema pueda usarlo, normalmente debes:


1. Ingresar a tu plataforma de facturación o contabilidad (Siigo, Alegra, Cifrato, etc.).
2. Ir al módulo de **habilitación o configuración de facturación electrónica** .
3. Buscar la opción de **sincronizar o consultar resoluciones asociadas** .
4. Confirmar que el prefijo aparece disponible y seleccionarlo como activo.
5. Cerrar sesión y volver a ingresar, para que el sistema recargue la nueva numeración.


Si cambias de proveedor tecnológico en el futuro, recuerda que debes **retirar la asociación anterior** en la DIAN antes de asociar la nueva, o el sistema puede generar conflictos entre resoluciones activas.


## Tabla resumen: pasos, dónde se hacen y tiempo estimado


Paso Dónde se realiza Tiempo estimado


Verificar resolución en estado definitivo Portal DIAN Inmediato (revisión)


Solicitar acceso con token al RUT Portal DIAN + correo electrónico 2-5 minutos


Ir a Configuración > Rangos de numeración Portal DIAN 1 minuto


Seleccionar proveedor tecnológico y prefijo Portal DIAN 2-5 minutos


Confirmar y verificar resoluciones asociadas Portal DIAN 1-2 minutos


Sincronizar resolución en el software contable Software de facturación 5-10 minutos


Habilitar fecha de salida a producción Portal DIAN (RUT, responsabilidad 52) 5 minutos


## Errores comunes al asociar prefijos (y cómo solucionarlos)


- **El proveedor tecnológico no aparece en la lista** : revisa que tu proveedor esté correctamente registrado ante la DIAN y que estés escribiendo el nombre exacto; si el problema persiste, contacta a tu proveedor para confirmar su razón social registrada.
- **El prefijo no aparece disponible para asociar** : confirma que la resolución ya esté en estado definitivo y no provisional; si acabas de recibir la aprobación, espera al menos 2-3 horas.
- **El token de acceso expiró** : solicita uno nuevo desde el inicio del proceso; no intentes reutilizar un enlace vencido.
- **Ya asociaste el prefijo pero no aparece en tu software** : además de asociarlo en la DIAN, debes sincronizarlo manualmente dentro de tu plataforma contable; revisa el módulo de habilitación electrónica.
- **Cambiaste de proveedor y la numeración antigua sigue activa** : retira la asociación anterior en la DIAN antes de asociar el nuevo proveedor, para evitar resoluciones duplicadas o rechazadas.
- **La factura se genera pero sin la numeración nueva** : valida que hayas completado también el paso de "fecha de salida a producción" en la sección de Habilitación, que actualiza tu RUT con la responsabilidad 52 (Facturador electrónico).


## Preguntas frecuentes


**¿Cuánto tiempo tarda en aprobarse la asociación de un prefijo?** El proceso dentro del portal de la DIAN es prácticamente inmediato una vez confirmas los datos. Lo que suele tomar más tiempo es la sincronización posterior con tu software contable y, si aplica, la espera recomendada de 2-3 horas tras la aprobación de la resolución.


**¿Puedo tener varios prefijos asociados al mismo tiempo?** Sí. Puedes asociar tantas resoluciones como manejes (factura de venta, POS, contingencia, exportación, etc.), repitiendo el proceso de selección y confirmación para cada una.


**¿Qué pasa si asocio el prefijo al proveedor tecnológico equivocado?** Debes ingresar nuevamente al portal, retirar esa asociación y repetir el proceso seleccionando el proveedor correcto. Mientras esto no se corrija, tu software real no podrá usar esa numeración.


**¿Es lo mismo "habilitación" que "asociar prefijos"?** No. La habilitación es el proceso completo mediante el cual te certificas ante la DIAN como facturador electrónico (incluye pruebas técnicas y aprobación). Asociar prefijos es un paso específico dentro (o después) de ese proceso, enfocado en vincular la numeración autorizada a tu proveedor tecnológico.


Asociar prefijos en la DIAN es un trámite corto, pero sensible: un dato mal seleccionado —el proveedor equivocado, una resolución aún provisional o un token vencido— puede bloquear tu facturación electrónica justo cuando más la necesitas. Sigue el orden correcto (RUT actualizado → token → Configuración → Rangos de numeración → proveedor → prefijo → sincronización en tu software) y verifica siempre el listado de resoluciones asociadas antes de dar el proceso por cerrado.


Si tu empresa maneja múltiples resoluciones, sedes o proveedores tecnológicos, automatizar la sincronización entre la DIAN y tu sistema contable —en lugar de hacerlo manualmente cada vez— reduce el riesgo de errores de numeración y te ahorra horas de configuración cada mes.
